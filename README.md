# anonymous_github_download
download anonymous github folder
1. 首先点开所有的folder，用F12打开并复制源代码<tree></tree>部分，保存到文件，如a.txt
2. 解析网页结构
```python
import os
from bs4 import BeautifulSoup
with open('a.txt','r') as f:
    html=f.readline()
# print(html)
soup=BeautifulSoup(html,'html.parser')
```
3. 开始下载
```python
import os
url='https://anonymous.4open.science'
def getFile(file):
    os.system('wget '+url+file.find('a').get('href'))
    pass
def getFolder(folder):
    if folder.contents[0].name=='a':
        dir_name=folder.contents[0].get_text()
        folder=folder.contents[1]
    else:
        dir_name='aaa'
        folder=folder.contents[0]
    os.makedirs(dir_name)
    cwd=os.getcwd()
    os.chdir(cwd+'/'+dir_name)
    # print(cwd+'/'+dir_name)
    for i in folder.contents:
        if i.name !='li':continue
        if 'folder' in i.get('class'):getFolder(i)
        else: getFile(i)
    os.chdir(cwd)
getFolder(soup.contents[0])
```
会创建一个aaa文件夹，所有文件按结构下载在aaa文件夹下
