### 调试与部署

```
wsl.exe -d Ubuntu
cd /mnt/e/文件夹/MyBlogs/blog

sudo bundle exec jekyll serve
sudo bundle exec jekyll serve --livereload --watch

http://127.0.0.1:4000/al-folio/

git add .
git commit -m 'u'
git push origin gh-pages
```


### 开发环境构建

- 安装WSL：`wsl --install -d Ubuntu`
- 进入环境并安装和调试

```
wsl.exe -d Ubuntu

cd /mnt/e/文件夹/MyBlogs/blog

sudo apt update

sudo apt install nodejs npm

sudo apt-get install ruby-full
sudo gem install bundler

sudo bundle install



(op)
# 如果报错 Failed to open TCP connection to medium.com:443 
注释_config.yaml的下面几行：
external_sources:
  - name: medium.com
    rss_url: https://medium.com/@al-folio/feed
  - name: Google Blog
    posts:
      - url: https://blog.google/technology/ai/google-gemini-update-flash-ai-assistant-io-2024/
        published_date: 2024-05-14

# 如果报错Imagemagick: Command returned pid 5247 exit 127 with error sh: 1: convert: not found
sudo apt-get update
sudo apt-get install imagemagick

# 如果报错/var/lib/gems/3.2.0/gems/jekyll-jupyter-notebook-0.0.6/lib/jekyll-jupyter-notebook/converter.rb:58:in spawn': No such file or directory - jupyter (Errno::ENOENT)
sudo apt update
sudo apt-get install jupyter

# 如果报错 cannot load such file -- jekyll-archives (LoadError)
在Gemfile的gem 'jekyll-archives-v2'前加一行gem 'jekyll-archives'
这样可能出现不兼容问题，出现的话注释掉'jekyll-archives-v2'
然后sudo bundle install
```



###  文档说明与功能调整

- 页面的导航栏布局没有单独的文件设置，而是通过`_pages`下面文件中的`nav: true`进行开启，然后设置`nav_order: 1`指定从左到右的顺序

- 默认页面是`blog.md`，但是内容在`about.md`里面；而格式在`_layouts/about.liquid`下（增加首页栏目也是在此处）

- `CUSTOMIZE.md`有极其详细的文档自定义说明

- **新增news**：在`_news`下添加文档即可，注意只会保留最近的5条在首页（在about.md中设置）

- **新增publication**：

  - `_bibliography/paper.bib`下直接加bib条目，abbr是期刊横幅缩写，bibtex_show点击展开bib，selected是否展示在首页，preview是将`asset/img/publication_preview`下的对应封面图链接

  - **新增期刊**：`_data/venues`新增期刊的横幅bar类型（其实目前就红蓝对应会议期刊）

  - **首页论文作者显示上限**：`_config.yml`的`max_author_limit`

  - **首页论文的左侧缩略图尺寸修改**：`_layouts/bib.liquid`中`<div class="col col-sm-3 abbr">`调整3增大可以让preview png整体更大；如果在此基础上调整高度，在`_sass\_base.scss`的下面文段中调整`height`即可

    ```
    
    .preview {
    width: 100%;
    max-width: 320px; /* 控制最大宽度 */
    height: 90px;    /* 统一高度 */
    overflow: hidden;
    }
    ```

    

  - pdf源文件默认目录是`asset/pdf`

  - 支持的paper.bib字段有：`abstract`, `altmetric`, `annotation`, `arxiv`, `bibtex_show`, `blog`, `code`, `dimensions`, `doi`, `eprint`, `html`, `isbn`, `pdf`, `pmid`, `poster`, `slides`, `supp`, `video`, and `website`.

  - 其他更多参见源码：

    - `abbr`: Adds an abbreviation to the left of the entry. You can add links to these by creating a venue.yaml-file in the \_data folder and adding entries that match.
    - `abstract`: Adds an "Abs" button that expands a hidden text field when clicked to show the abstract text
    - `altmetric`: Adds an [Altmetric](https://www.altmetric.com/) badge (Note: if DOI is provided just use `true`, otherwise only add the altmetric identifier here - the link is generated automatically)
    - `annotation`: Adds a popover info message to the end of the author list that can potentially be used to clarify superscripts. HTML is allowed.
    - `arxiv`: Adds a link to the Arxiv website (Note: only add the arxiv identifier here - the link is generated automatically)
    - `bibtex_show`: Adds a "Bib" button that expands a hidden text field with the full bibliography entry
    - `blog`: Adds a "Blog" button redirecting to the specified link
    - `code`: Adds a "Code" button redirecting to the specified link
    - `dimensions`: Adds a [Dimensions](https://www.dimensions.ai/) badge (Note: if DOI or PMID is provided just use `true`, otherwise only add the Dimensions' identifier here - the link is generated automatically)
    - `html`: Inserts an "HTML" button redirecting to the user-specified link
    - `pdf`: Adds a "PDF" button redirecting to a specified file (if a full link is not specified, the file will be assumed to be placed in the /assets/pdf/ directory)
    - `poster`: Adds a "Poster" button redirecting to a specified file (if a full link is not specified, the file will be assumed to be placed in the /assets/pdf/ directory)
    - `slides`: Adds a "Slides" button redirecting to a specified file (if a full link is not specified, the file will be assumed to be placed in the /assets/pdf/ directory)
    - `supp`: Adds a "Supp" button to a specified file (if a full link is not specified, the file will be assumed to be placed in the /assets/pdf/ directory)
    - `website`: Adds a "Website" button redirecting to the specified link

    - You can implement your own buttons by editing the [\_layouts/bib.liquid](_layouts/bib.liquid) file.

- **新增修改首页内容**（增、减、顺序-不纳入bar/page）：首页布局全在`_layouts/about.liquid`，例如想新增一个openings，直接在`_layouts/about.liquid`的line45-47打开Openings的段落；再如想在最下面加小地图，就把地图文段复制到该文件的最后部分：

  ```
  <div style="width: 350px; height: 85px; margin: 0 auto;">  
    <script 
      type="text/javascript" 
      id="clustrmaps" 
      src="//cdn.clustrmaps.com/map_v2.js?cl=ffffff&w=a&t=tt&d=QsYwRxkSw7tD_Z207HseBeBR2zeE7dzDIBvYiWNWnCg">
    </script>
  </div>	
  ```
 
- **删掉脚注的bar**：`_layouts/default.liquid`把定义脚注的bar的文件` {% include footer.liquid %}`给删了不include就行，layout就是管页面布局这个的

### 疑难杂症

- pdf文件的permission denied：可能是读写权限限制，先查看所有文件的权限`ls -l "/mnt/e/文件夹/MyBlogs/blog/al-folio-main/al-folio-main/_site/assets/pdf"`，如果存在权限不一样的，附上写权限`chmod -R +w /mnt/e/文件夹/MyBlogs/blog/al-folio-main/al-folio-main/_site/assets/pdf`

### TBD

google_scholar: false先设置不搜索，不然太卡了，后面更新一下citation

Qi的黑体去掉

修改所有的paper link, pub的源文件上传，可以google drive(pdf, download,区分一下哪个是下载)

怎么加CCFA的badge
