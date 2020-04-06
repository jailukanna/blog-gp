---
title: mysql example willie-chess (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'willie-chess'

Functions in program: 
* `def moveChessPiece(b, player, s, d):`
* `def chessmove(bot, trigger, cnx, args):`
* `def chessshow(bot, trigger, cnx, args):`
* `def colortest(bot, trigger):`
* `def chessgames(bot, trigger, cnx, activeGameID):`
* `def chesschallenge(bot, trigger, cnx, args):`
* `def chess(bot, trigger, cnx):`
* `def requireDatabase(func):`
* `def namePiece(c):`

Modules used in program: 
* `import mysql.connector`
* `import willie, phpserialize`

## python willie-chess

Python mysql example: willie-chess

```python
# coding=utf8
"""
willie-chess.py - chess, duh
"""

# whipped this up at lunch.
# clone https://github.com/embolalia/willie
# put this in ./willie/modules
# create mysql database and table "chessgame"
# all moves are validated but en passant and castling aren't implemented
# also check and checkmate are not implemented

# Oh, I guess that I also forgot the create table statement, but I'm sure
# it goes something like this:
#
# create table chessgame (
#   id int primary key auto_increment
# , whiteNick char(32) not null
# , blackNick char(32) not null
# , numMoves int not null
# , state enum('white to move', 'black to move', other unimplemented game states...)
# , board char(64) not null default 'rbhqkhbrpppppppp you figure out the rest, caps for other color'
# , castlingPiecesUnmoved char(12) not null default 'h1h5h8a1a5a8' /* or is it s/5/6 idr; castling isn't even implemented who cares */
# );
 
# TODO determine why i should care about threading wrt mysql connector
 
from __future__ import unicode_literals
import willie, phpserialize
from willie.module import commands, example, NOLIMIT
from willie.formatting import colors, color
import mysql.connector
from mysql.connector import errorcode
 
mysqlConfig = {
  'user':               'mysqluser'
, 'password':           'mysqlpassword'
, 'host':               '127.0.0.1'
# 'host':               'web-server'
, 'database':           'chess'
, 'raise_on_warnings':  True
}
 
# key=nick, value=gameID
# if a command is issued, it can infer game ID from this, the last referred to game
activeGameByNick = {}
 
def namePiece(c):
  c = c.tolower()
  if   c == 'p': return 'pawn'
  elif c == 'r': return 'rook'
  elif c == 'h': return 'knight'
  elif c == 'b': return 'bishop'
  elif c == 'q': return 'queen'
  elif c == 'k': return 'king'
  elif c == ' ': return 'empty square'
  else: return 'unknown piece "%s"' % c
 
def requireDatabase(func):
  def thunk(bot, trigger):
    try:
      cnx = mysql.connector.connect(**mysqlConfig)
      return func(bot, trigger, cnx)
    finally:
      if cnx: cnx.close()
  return thunk
 
@commands('chess')
@example('.chess challenge <opponent-nick>') # only the first example seems to work
@example('.chess show <game-id>')
@example('.chess list')
@example('.chess move <game-id> <from-space-id> <to-space-id>')
@willie.module.thread(False) # Run in main thread?
@requireDatabase
def chess(bot, trigger, cnx):
  '''Every chess command'''
 
  if trigger.group(2) is None:
    bot.reply('examples of how to use .chess command:')
    bot.reply('.chess challenge <opponent-nick>')
    bot.reply('.chess show <game-id>')
    bot.reply('.chess list')
    bot.reply('.chess move <game-id> <from-space-id> <to-space-id>')
    return
 
  args = trigger.group(2).lower().split()
  activeGameID = activeGameByNick[trigger.nick] if trigger.nick in activeGameByNick else None
 
  if args[0] == 'challenge':
    chesschallenge(bot, trigger, cnx, args)
    return
 
  if args[0] == 'list':
    chessgames(bot, trigger, cnx, activeGameID)
    return
 
  if args[0] == 'show':
    if len(args) == 1:
      args.append(activeGameID)
    chessshow(bot, trigger, cnx, args)
    return
 
  if args[0] == 'castle':
    bot.reply('castling not yet supported')
  else:
    if len(args) < 3:
      raise ValueError('not enough arguments')
    if len(args) == 3:
      args = [args[0], activeGameID, args[1], args[2]]
    if len(args) != 4:
      raise ValueError('Wrong number of arguments')
    chessmove(bot, trigger, cnx, args)
   
def chesschallenge(bot, trigger, cnx, args):
  """Begin a new game of chess with named player."""
  whiteNick = args[1]
  if whiteNick:
    blackNick = str(trigger.nick).lower()
    cursor = cnx.cursor()
    cursor.execute('''
      INSERT INTO chessgame (
        whiteNick
      , blackNick
      , board
      ) VALUES (
        %s
      , %s
      , 'rhbqkbhrpppppppp                                PPPPPPPPRHBQKBHR'
      )
    ''', [whiteNick, blackNick])
    cnx.commit()
    cursor.close()
    bot.say('%s has been challenged to game #%d by %s' % (whiteNick, cursor.lastrowid, blackNick))
 
def chessgames(bot, trigger, cnx, activeGameID):
  """Enumerate the games you are playing."""
  nick = str(trigger.nick).lower()
  cursor = cnx.cursor()
  cursor.execute('''
    SELECT
      id
    , whiteNick
    , blackNick
    , state
    , numMoves
    FROM chessgame
    WHERE
      (whiteNick = %s OR blackNick = %s)
      AND (state  = 'white to move' OR state = 'black to move')
  ''', [nick, nick])
  for row in cursor:
    gameID = row[0]
    whiteNick = row[1]
    blackNick = row[2]
    state = row[3]
    numMoves = row[4]
    a = '*' if gameID == activeGameID else ' '
    bot.reply('%c % 3d moves | game id: % 3d | %s | %8s (white) vs. %8s (black)' % (a, numMoves, gameID, state, whiteNick, blackNick))
  cursor.close()
 
@commands('colortest')
def colortest(bot, trigger):
  for bg in range(0,99):
    line = '%02d: ' % bg
    for fg in range(0,99):
      fgs = '%02d' % fg
      line += color(fgs  + willie.formatting.bold(fgs), bg, fg)
    bot.reply(line)
 
def chessshow(bot, trigger, cnx, args):
  print('args:', args)
  """See the state of the identified game."""
  gameID = int(args[1])
  if gameID:
    cursor = cnx.cursor()
    cursor.execute('''
      SELECT
        board
      , whiteNick
      , blackNick
      , state
      , numMoves
      FROM chessgame
      WHERE
        id = %s
    ''', [gameID])
    width = 26
    for row in cursor: # lel only one result at most
      bot.reply('Game #%d | %d moves | %s' % (gameID, row[4], row[3]))
      s = row[2] + ' (black)'
      bot.reply((width - len(s)) / 2 * ' ' + s)
      bot.reply('   A  B  C  D  E  F  G  H')
      board = row[0]
      for i in range(8):
        line = str(8-i) + ' '
        x = -1
        for c in row[0][i*8:i*8+8]:
          x += 1
          Cc = c.lower()
          if Cc == 'h':
            Cc = 'k'
          elif Cc == 'k':
            Cc = '&'
          line += color(' %s ' % willie.formatting.bold(Cc), colors.MAROON if c.isupper() else 15, colors.BLACK if (x+i)%2 else 8)
        bot.reply(line)
      s = row[1] + ' (white)'
      bot.reply((width - len(s)) / 2 * ' ' + s)
    cursor.close()
 
def chessmove(bot, trigger, cnx, args):
  """Make a move."""
  try:
    discard, gameID, src, dst = args
    gameID = int(gameID)
    assert(len(src) == 2 and len(dst) == 2 and 'a' <= src[0] and 'a' <= dst[0] and src[0] <= 'h' and dst[0] <= 'h' and '0' <= src[1] and '0' <= dst[1] and src[1] <= '8' and dst[1] <= '8')
  except Exception as e:
    bot.reply('invalid arguments (maybe forgot the game ID?)')
    return
  srcFile = ord(src[0]) - ord('a') # source x
  dstFile = ord(dst[0]) - ord('a') # destination x
  srcRank = 8 - int(src[1]) # source y
  dstRank = 8 - int(dst[1]) # destination y
  bot.reply('trying to move (%d,%d) to (%d,%d)' % (srcFile, srcRank, dstFile, dstRank)) # sx, sy, dx, dy
   
  cursor = cnx.cursor()
  cursor.execute('''
    SELECT
      board
    , whiteNick
    , blackNick
    , state
    , numMoves
    , castlingPiecesUnmoved
    FROM chessgame
    WHERE
      id = %s
  ''', [gameID])
  moved = False
  row = cursor.fetchone()
  if not row:
    bot.reply('could not find game #%d' % gameID)
    return
  activeGameByNick[trigger.nick] = gameID
  playerMoving = False
  if (str(trigger.nick).lower() == row[1] and row[3] == 'white to move'):
    playerMoving = 'w'
  elif (str(trigger.nick).lower() == row[2] and row[3] == 'black to move'):
    playerMoving = 'b'
  if playerMoving:
    bot.reply('game %s %d moves %s %s (WHITE) vs. %s (black)' % (gameID, row[4], row[3], row[1], row[2]))
    board = moveChessPiece(row[0], playerMoving, (srcFile, srcRank), (dstFile, dstRank))
    castlingPiecesUnmoved = row[5]
    if src.upper() in row[5]:
      castlingPiecesUnmoved = castlingPiecesUnmoved.replace(src.upper(),'')
    sql = '''
      UPDATE chessgame
      SET
        board = %s
      , state = if(state = 'white to move', 'black to move', 'white to move')
      , numMoves = numMoves + 1
      , castlingPiecesUnmoved = %s
      WHERE
        id = %s
    '''
    print(sql)
    cursor.execute(sql, [board, castlingPiecesUnmoved, gameID])
  else:
    bot.reply('it is not your turn to move in game #%d' % gameID)
    return
 
  cnx.commit()
  cursor.close()
 
# args: board, player, source, destination
# b = board string
# player = 'w' | 'b'
# s = (x, y) source square coordinates
# d = (x, y) destination square coordinates
def moveChessPiece(b, player, s, d):
  # Validate that there is movement
  if s == d:
    return ValueError('Flag is not moving. Wind is not moving. Mind is not moving.')
  # Split string into list
  b = list(b)
  # Extract pieces
  Pc  = b[s[1] * 8 + s[0]]
  Dpc = b[d[1] * 8 + d[0]]
  # Validate the piece
  if Pc == ' ':
    raise ValueError("If you call this a piece, you deny its present existence.")
  # Validate piece ownership
  PcWhite = Pc.isupper()
  DpcWhite = not Dpc.islower() # lel
  capturing = (Dpc != ' ') and (PcWhite != DpcWhite)
  if (player == 'w') != PcWhite:
    raise ValueError("You cannot move your opponent's pieces!")
  # Validate the movement
  pc = Pc.lower()
  dx = d[0]-s[0]
  dy = d[1]-s[1]
 
  print('source piece:', Pc)
  print('destination piece:', Dpc)
  print('capturing:', capturing)
 
  if pc == 'k': # king
    if abs(dx) + abs(dy) != 1:
      raise ValueError("Piece cannot move that way")
  elif pc == 'q': # queen
    if (abs(dx) != abs(dy)) and (dx) and (dy):
      raise ValueError("Piece cannot move that way")
  elif pc == 'r': # rook
    if (dx) and (dy):
      raise ValueError("Piece cannot move that way")
  elif pc == 'b': # bishop
    if abs(dx) != abs(dy):
      raise ValueError("Piece cannot move that way")
  elif pc == 'h': # knight
    if (abs(dx) + abs(dy) != 3) or dx == 0 or dy == 0:
      raise ValueError("Piece cannot move that way")
  elif pc == 'p':
    if (abs(dy) > 2) or ((dy > 0) == PcWhite) or (abs(dx) > 1) or (abs(dx) != capturing) or ((abs(dy) == 2) and (dx or (s[1] != 6 if PcWhite else s[1] != 1))):
      raise ValueError("Piece cannot move that way")
  # TODO castling
  # check for mid-flight collisions
  if pc != 'h':
    dxy = dx or dy
    cx = 1 if dx > 0 else (-1 if dx < 0 else 0)
    cy = 1 if dy > 0 else (-1 if dy < 0 else 0)
    print('collision check:', dxy, cx, cy)
    for i in xrange(1, dxy) if dxy > 0 else xrange(1, -dxy):
      print('   check', i, s[0] + i * cx, s[1] + i * cy)
      if b[(s[1] + i * cy) * 8 + s[0] + i * cx] != ' ':
        raise ValueError('There is another piece in the way')
   
  print('move succeeds')
 
  b[d[1] * 8 + d[0]] = Pc
  b[s[1] * 8 + s[0]] = ' '
  return ''.join(b) 


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
