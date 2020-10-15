# WSL使用win10浏览器

- 以Chrome为例

1. 在`/usr/bin/`创建win下的chrome的软连接
```bash
 sudo ln -s /mnt/c/Program\ Files\ \(x86\)/Google/Chrome/Application/chrome.exe /usr/bin/google-chrome
```
2. 再在`/etc/alternatives`下创建链接到之前软链接的软链接
```bash
cd /etc/alternatives/
sudo mv gnome-www-browser gnome-www-browser-bkp
sudo ln -s /usr/bin/google-chrome gnome-www-browser
```
3. 用软连接替换默认浏览器
```bash
sudo mv x-www-browser x-www-browser-bkp
sudo ln -s /usr/bin/google-chrome x-www-browser

```
## jupyter的设置
1. 编辑jupyter的配置文件

```bash
vim $HOME/.jupyter/jupyter_notebook_config.py
```
2. 加入如下配置
```python
import webbrowser
webbrowser.register('google-chrome', None, webbrowser.GenericBrowser(u'/usr/bin/google-chrome'))
c.NotebookApp.browser = 'google-chrome'

c.NotebookApp.use_redirect_file = False
```
