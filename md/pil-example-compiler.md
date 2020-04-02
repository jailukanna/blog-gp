---
title: pil example compiler (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'compiler'

Functions in program: 
* `def image_process(pil_img, border):`
* `def prob_statement(contest_path, prob_num):`

Modules used in program: 
* `import sympy`
* `import urllib`
* `import re`
* `import os`
* `import requests`

## python compiler

Python pil example: compiler

```python
from bs4 import BeautifulSoup
import requests
import os
from PIL import Image
import re
import urllib
import sympy


def prob_statement(contest_path, prob_num):
    accepted_list = []
    with open(os.path.join(contest_path, str(prob_num) + ".html"), "r", encoding="utf8") as f:

        contents = f.read()

        soup = BeautifulSoup(contents, 'html.parser')
    # page = requests.get(url)
    # soup = BeautifulSoup(page.text, 'xml')
    paragraph = ""
    image_latex = []
    for element in soup.find(class_="mw-content-ltr").find(class_="mw-parser-output"):
        mini_image_latex = []
        mini_paragraph = ""
        if element.name == 'h2' or element.name == 'h3':
            if element.find(class_="mw-headline", recursive=False).text.lower().find("problem") == -1:
                break
        if element.name == 'ul' or element.name == 'ol':
            paragraph += r' \begin{itemize}'
            bullets = element.findAll('li')
            for bullet in bullets:
                mini_paragraph = ""
                mini_image_latex = []
                for link in bullet.findAll('a'):
                    link.replaceWithChildren()
                test = bullet.findAll('img', recursive=False)
                for s in test:
                    if str(s['alt']).find("[asy]") != -1:
                        accepted_list.append(s['src'])
                        mini_image_latex.append("")
                    elif str(s['alt']).find("$") == -1:
                        if s.has_attr("class") and s['class'] == ['latexcenter']:
                            mini_image_latex.append(r" \begin{center} " + s['alt'] + " \end{center} ")
                        else:
                            mini_image_latex.append("")
                    else:
                        mini_image_latex.append(s['alt'])
                mini_paragraph += r" \item "
                mini_paragraph += str(bullet)
                mini_paragraph = str(mini_paragraph).replace("<p>", "")
                mini_paragraph = str(mini_paragraph).replace("</p>", "")
                mini_paragraph = str(mini_paragraph).replace("</img>", "")
                mini_paragraph = str(mini_paragraph).replace("<i>", "")
                mini_paragraph = str(mini_paragraph).replace("</i>", "")
                mini_paragraph = str(mini_paragraph).replace("<li>", "")
                mini_paragraph = str(mini_paragraph).replace("</li>", "")
                mini_paragraph = str(mini_paragraph).replace("</b>", "")
                mini_paragraph = str(mini_paragraph).replace("<b>", "")
                mini_paragraph = str(mini_paragraph).replace("<ul>", "")
                mini_paragraph = str(mini_paragraph).replace("</ul>", "")
                regex = re.compile(".*?<(.*?)>")
                i = 0
                for s in re.findall(regex, mini_paragraph):
                    mini_paragraph = mini_paragraph.replace("<" + s + ">", mini_image_latex[i])
                    i += 1
                if bullet.find('a'):
                    if bullet.find('a').has_attr("class"):  # standalone iamge
                        if bullet.find('a')['class'] == ['image']:
                            link_source = bullet.find('a').img['src']
                            accepted_list.append(link_source)
                image_latex += mini_image_latex
                paragraph += mini_paragraph
            paragraph += r' \end{itemize}'
        if element.name == 'p' or element.name == 'center':
            for link in element.findAll('a'):
                link.replaceWithChildren()
            for s in element.findAll('img'):
                if str(s['alt']).find("[asy]") != -1:
                    accepted_list.append(s['src'])
                    mini_image_latex.append("")
                elif str(s['alt']).find("$") == -1:
                    if s.has_attr("class") and s['class'] == ['latexcenter']:
                        mini_image_latex.append(r" \begin{center} " + s['alt'] + " \end{center} ")
                    else:
                        mini_image_latex.append("")
                        accepted_list.append(s['src'])
                else:
                    mini_image_latex.append(s['alt'])
            mini_paragraph += str(element)
            mini_paragraph = str(mini_paragraph).replace("<p>", "")
            mini_paragraph = str(mini_paragraph).replace("</p>", "")
            mini_paragraph = str(mini_paragraph).replace("</img>", "")
            mini_paragraph = str(mini_paragraph).replace("<i>", "")
            mini_paragraph = str(mini_paragraph).replace("</i>", "")
            mini_paragraph = str(mini_paragraph).replace("</b>", "")
            mini_paragraph = str(mini_paragraph).replace("<b>", "")
            mini_paragraph = str(mini_paragraph).replace("<br>", "")
            mini_paragraph = str(mini_paragraph).replace("<br/>", "")
            mini_paragraph = str(mini_paragraph).replace("<center>", "")
            mini_paragraph = str(mini_paragraph).replace("</center>", "")
            mini_paragraph = str(mini_paragraph).replace("<small>", "")
            mini_paragraph = str(mini_paragraph).replace("</small>", "")
            mini_paragraph = str(mini_paragraph).replace("<div>", "")
            mini_paragraph = str(mini_paragraph).replace("</div>", "")
            mini_paragraph = str(mini_paragraph).replace("<tt>", "")
            mini_paragraph = str(mini_paragraph).replace("</tt>", "")
            regex = re.compile(".*?<(.*?)>")
            i = 0
            for s in re.findall(regex, mini_paragraph):
                mini_paragraph = mini_paragraph.replace("<" + s + ">", mini_image_latex[i])
                i += 1
            if element.find('a'):
                if element.find('a').has_attr("class"):
                    if element.find('a')['class'] == ['image']:
                        link_source = element.find('a').img['src']
                        accepted_list.append(link_source)
            image_latex += mini_image_latex
            paragraph += mini_paragraph
    return paragraph, accepted_list


def image_process(pil_img, border):
    width, height = pil_img.size
    new_width = width + border * 2
    new_height = height + border * 2
    result = Image.new(pil_img.mode, (new_width, new_height))
    result.paste(pil_img, (border, border))
    return result

#
MAIN_PATH = os.path.join(os.path.dirname(os.path.realpath(__file__)), "image_folder")
AMC_PATH = os.path.join(MAIN_PATH, "AMC")
AIME_PATH = os.path.join(MAIN_PATH, "AIME")
USAJMO_PATH = os.path.join(MAIN_PATH, "USAJMO")
USAMO_PATH =  os.path.join(MAIN_PATH, "USAMO")
os.makedirs(os.path.dirname(AMC_PATH),exist_ok=True)
os.makedirs(os.path.dirname(AIME_PATH),exist_ok=True)
os.makedirs(os.path.dirname(USAJMO_PATH),exist_ok=True)
os.makedirs(os.path.dirname(USAMO_PATH),exist_ok=True)

#AMC Problems from 2000 to 2001
#Only two per year: AMC 10 and AMC 12
#AMC Problems from 2002 to 2019
#Four per year: 10A, 10B, 12A, 12B
#
for year in range(2000, 2020):
    if year < 2002:
        versions = ["10", "12"]
    else:
        versions = ["10A", "10B", "12A", "12B"]
    os.makedirs(os.path.join(AMC_PATH, str(year)), exist_ok=True)
    for version in versions:

        CONTEST_PATH = os.path.join(AMC_PATH, str(year),version)
        os.makedirs(CONTEST_PATH, exist_ok=True)
        for problem_num in range(1,26):
            URL = f"https://artofproblemsolving.com/wiki/index.php/{year}_AMC_{version}_Problems/Problem_{problem_num}"
            try:
                latex, images = prob_statement(os.path.join(os.path.dirname(os.path.realpath(__file__)), "webpages\\AMC\\"+str(year)+"\\"+version), problem_num)
                latex = latex.replace("\n", "\n \n")
                #print(latex)
                PROBLEM_PATH = os.path.join(CONTEST_PATH, str(problem_num))
                os.makedirs(PROBLEM_PATH, exist_ok=True)
    #             #TODO: write latex to latex.txt in os.path.join(CONTEST_PATH, "latex.txt")
                txt = open(os.path.join(PROBLEM_PATH, "latex.txt"), "w+")
                txt.write(latex)
                txt.close()
                # print(PROBLEM_PATH)
                # print(latex)
                STATEMENT_PATH = os.path.join(PROBLEM_PATH, "statement.png")

                sympy.preview(latex, viewer='file',
                              filename=STATEMENT_PATH, euler=False,
                              dvioptions=["-T", "tight", "-z", "0", "--truecolor", "-D 600", "-bg", "Transparent", "-fg",
                                          "White"])
                im = Image.open(STATEMENT_PATH)
                im = image_process(im, 10)
                im.save(STATEMENT_PATH)
                # display_surface.blit(im, (0, 0))
                # for event in pygame.event.get():
                #     if event.type == pygame.QUIT:
                #         pygame.quit()
                #         quit()
                #     pygame.display.update()
                #     pygame.display.flip()

                IMAGE_FOLDER = os.path.join(PROBLEM_PATH, "images")
                os.makedirs(IMAGE_FOLDER, exist_ok=True)
                IMAGE_INDEX = 0
                for image in images:
                    if image.startswith('//latex'):
                        image = "https:" + image
                    IMAGE_PATH = os.path.join(IMAGE_FOLDER, str(IMAGE_INDEX) + ".png")
                    urllib.request.urlretrieve(image, IMAGE_PATH)
                    pil_image = Image.open(IMAGE_PATH)
                    layer = Image.new('RGB', pil_image.size, (255, 255, 255))
                    layer.paste(pil_image, (0, 0))
                    layer.save(IMAGE_PATH)
                    IMAGE_INDEX += 1


            except Exception as e:
                print("problem " + str(problem_num) + " on the " + str(year) + "AMC " + version)


# #AIME Problems: From 1983 to 1999, there was only one contest per year. From 2000 to present, there has been an AIME I
# # and an AIME II. 15 problems per contest.

# recompile: 1984 AIME 15 done, 1989 AIME 14 done, 1991 AIME 6 done, 1991 AIME 12 done, 1993 AIME 6 done, 2005 AIME I p13 done,
#  2008 aime I p13 done, 2008 AIME II p15 done, 2009 AIME I P9 done, 2012 AIME I P15 done, 2014 AIME II P11 done ,
# 2015 AIME I P15 done
# for year in range(2015, 2016):
#
#     if year < 2000:
#         versions = [""]
#     else:
#         versions = ["_I", "_II"]
#     for version in versions:
#         if version == "":
#             CONTEST_PATH = os.path.join(AIME_PATH, str(year))
#         elif version == "_I":
#             CONTEST_PATH = os.path.join(AIME_PATH, str(year), "1")
#         else:
#             CONTEST_PATH = os.path.join(AIME_PATH, str(year), "2")
#         os.makedirs(CONTEST_PATH, exist_ok=True)
#         for problem_num in range(1, 16):
#             if problem_num != 15 or version == "_II":
#                 continue
#
#             images = []
#
#             PROBLEM_PATH = os.path.join(CONTEST_PATH, str(problem_num))
#             os.makedirs(PROBLEM_PATH, exist_ok=True)
#             # TODO: write latex to latex.txt in os.path.join(CONTEST_PATH, "latex.txt")
#             txt = open(os.path.join(PROBLEM_PATH, "latex.txt")).read()
#             print(txt)
#             print(PROBLEM_PATH)
#             os.makedirs(PROBLEM_PATH, exist_ok=True)
#             STATEMENT_PATH = os.path.join(PROBLEM_PATH, "statement.png")
#             sympy.preview(txt, viewer='file',
#                           filename=STATEMENT_PATH, euler=False,
#                           dvioptions=["-T", "tight", "-z", "0", "--truecolor", "-D 600", "-bg", "Transparent", "-fg",
#                                       "White"])
#             im = Image.open(STATEMENT_PATH)
#             im = image_process(im, 10)
#             im.save(STATEMENT_PATH)
#             IMAGE_FOLDER = os.path.join(PROBLEM_PATH, "images")
#             os.makedirs(IMAGE_FOLDER, exist_ok=True)
#             IMAGE_INDEX = 0
#             for image in images:
#                 if image.startswith('//latex'):
#                     image = "https:" + image
#                 IMAGE_PATH = os.path.join(IMAGE_FOLDER, str(IMAGE_INDEX) + ".png")
#                 urllib.request.urlretrieve(image, IMAGE_PATH)
#                 pil_image = Image.open(IMAGE_PATH)
#                 layer = Image.new('RGB', pil_image.size, (255, 255, 255))
#                 layer.paste(pil_image, (0, 0))
#                 layer.save(IMAGE_PATH)
#                 IMAGE_INDEX += 1
# # #USAJMO Problems: Started from 2010, only one contest per year, 6 problems.
# # for year in range(2017, 2020):
# #     CONTEST_PATH = os.path.join(USAJMO_PATH, str(year))
# #     os.makedirs(CONTEST_PATH, exist_ok=True)
# #
# #     for problem_num in range(1, 7):
# #         URL = f"https://artofproblemsolving.com/wiki/index.php/{year}_USAJMO_Problems/Problem_{problem_num}"
# #         if not (year == 2013 and problem_num == 3):
# #             latex, images = prob_statement(URL)
# #         else:
# #             latex, images = prob_statement("https://artofproblemsolving.com/wiki/index.php/2013_USAMO_Problems/Problem_1")
# #         PROBLEM_PATH = os.path.join(CONTEST_PATH, str(problem_num))
# #         os.makedirs(PROBLEM_PATH, exist_ok=True)
# #         # TODO: write latex to latex.txt in os.path.join(CONTEST_PATH, "latex.txt")
# #         txt = open(os.path.join(PROBLEM_PATH, "latex.txt"), "w+")
# #         txt.write(latex)
# #         txt.close()
#         print(PROBLEM_PATH)
#         print(latex)
#
#         STATEMENT_PATH = os.path.join(PROBLEM_PATH, "statement.png")
#         sympy.preview(latex, viewer='file',
#                       filename=STATEMENT_PATH, euler=False,
#                       dvioptions=["-T", "tight", "-z", "0", "--truecolor", "-D 600", "-bg", "Transparent", "-fg",
#                                   "White"])
#         im = Image.open(STATEMENT_PATH)
#         im = image_process(im, 10)
#         im.save(STATEMENT_PATH)
#         IMAGE_FOLDER = os.path.join(PROBLEM_PATH, "images")
#         os.makedirs(IMAGE_FOLDER, exist_ok=True)
#         IMAGE_INDEX = 0
#         for image in images:
#             if image.startswith('//latex'):
#                 image = "https:" + image
#             IMAGE_PATH = os.path.join(IMAGE_FOLDER, str(IMAGE_INDEX) + ".png")
#             urllib.request.urlretrieve(image, IMAGE_PATH)
#             pil_image = Image.open(IMAGE_PATH)
#             layer = Image.new('RGB', pil_image.size, (255, 255, 255))
#             layer.paste(pil_image, (0, 0))
#             layer.save(IMAGE_PATH)
#             IMAGE_INDEX += 1
#USAMO Problems: Started from 1972, only one contest per year, 5 problems from 1972 to 1995, 6 problems from 1996 to present.
# for year in range(2017, 2020):
#     CONTEST_PATH = os.path.join(USAMO_PATH, str(year))
#     os.makedirs(os.path.dirname(CONTEST_PATH),exist_ok=True)
#     if year <= 1995:
#         max_probs = 6
#     else:
#         max_probs = 7
#     for problem_num in range(1, max_probs):
#         URL = f"https://artofproblemsolving.com/wiki/index.php/{year}_USAMO_Problems/Problem_{problem_num}"
#         if not (year == 2017 and problem_num == 3):
#             latex, images = prob_statement(URL)
#         else:
#             latex = "No solution."
#             images = []
#         PROBLEM_PATH = os.path.join(CONTEST_PATH, str(problem_num))
#
#         print(PROBLEM_PATH)
#         print(latex)
#         latex = latex.replace('ă','a')
#         os.makedirs(PROBLEM_PATH, exist_ok=True)
#         # TODO: write latex to latex.txt in os.path.join(CONTEST_PATH, "latex.txt")
#         txt = open(os.path.join(PROBLEM_PATH, "latex.txt"), "w+")
#         txt.write(latex)
#         txt.close()
#         STATEMENT_PATH = os.path.join(PROBLEM_PATH, "statement.png")
#         sympy.preview(latex, viewer='file',
#                       filename=STATEMENT_PATH, euler=False,
#                       dvioptions=["-T", "tight", "-z", "0", "--truecolor", "-D 600", "-bg", "Transparent", "-fg",
#                                   "White"])
#         im = Image.open(STATEMENT_PATH)
#         im = image_process(im, 10)
#         im.save(STATEMENT_PATH)
#         IMAGE_FOLDER = os.path.join(PROBLEM_PATH, "images")
#         os.makedirs(IMAGE_FOLDER,exist_ok=True)
#         IMAGE_INDEX = 0
#         for image in images:
#             if image.startswith('//latex'):
#                 image = "https:" + image
#             IMAGE_PATH = os.path.join(IMAGE_FOLDER, str(IMAGE_INDEX) + ".png")
#             urllib.request.urlretrieve(image, IMAGE_PATH)
#             pil_image = Image.open(IMAGE_PATH)
#             layer = Image.new('RGB', pil_image.size, (255, 255, 255))
#             layer.paste(pil_image, (0, 0))
#             layer.save(IMAGE_PATH)
#             IMAGE_INDEX += 1


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
