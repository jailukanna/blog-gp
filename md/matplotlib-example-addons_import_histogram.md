---
title: matplotlib example addons import histogram (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'addons import histogram'


Modules used in program: 
* `import re`
* `import matplotlib.pyplot as plt`

## python addons import histogram

Python matplotlib example: addons import histogram

```python
#!/usr/bin/python
from github import Github
from collections import Counter
import matplotlib.pyplot as plt
import re

# Set this to a personal access token.
# - Select "repo" for the oauth scopes.
# See https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/
g = Github('YOUR_TOKEN')

repositories = set()

# Note: Gets rate limited and fails if too many hits
content_files = g.search_code(query='code:tensorflow_addons', highlight=True)
i = 0
api_histogram= Counter()
for content in content_files:
    # print(content.text_matches)
    if (content.repository.full_name=="https://github.com/tensorflow/addons/"): continue
    for text_match in content.text_matches:
        match = re.search(
            '(from|import)\s+tensorflow_addons\.*(\w+)?(\s+import\s+(\w+))?', text_match["fragment"])
        if match:
            if ( not match.group(2) and not match.group(4)):
                print(match.group())
                api_histogram['tensorflow_addons']+=1
            if match.group(2):
                print(match.group(2))
                api_histogram[match.group(2)]+=1
            if match.group(4):
                print(match.group(4))
                api_histogram[match.group(4)]+=1
    repositories.add(content.repository.full_name)
    if (len(repositories)==100): break
    rate_limit = g.get_rate_limit()
    if rate_limit.search.remaining == 0:
        print('WARNING: Rate limit on searching was reached.  Results are incomplete.')
        break
fig = plt.figure(figsize=(15, 10))
plt.title("Tensorflow addon import histogram on Github repos code search")
plt.bar(list(api_histogram.keys()), api_histogram.values(), width=0.8, color='g')
fig.savefig('plot.png')
for repo in sorted(repositories):
    print(repo)
print("Total Repos: %d" % len(repositories))

rate_limit = g.get_rate_limit()
print('Search rate limit:')
print(rate_limit.search)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
