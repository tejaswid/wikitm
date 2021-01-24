# How to develop this website?
This file presents an overview of the layout of the website and describes how to add content to the website.

## File and Folder Description

### Files
| File | Description |
| -------------- | ----------- |
| README.md | Readme file for this website |
| License.md | License file |
| creating.md | The file you are currently reading |
| about.md | File to provide atom feed (like RSS). Programs that keep track of updates to this website will use this feed |
| _config.yml | File where the website is configured |
| Gemfile | File where Ruby dependencies called Gems are specified |
| index.html | The home page of the website |
| 404.html | The 404 page of the website |
| atom.xml | For Atom feed |
| styles.scss | Imports the .scss (CSS with superpowers) files in _sass to create the final css for the webpage |

### Folders
| Folder | Description |
| -------------- | ----------- |
| _includes | Contains files that define the header, sidebar, title-tab, footer etc. |
| _layouts | Contains files that describe different page layouts |
| _sass | Contains files that describe the SCSS for different parts of the website |
| assets | Folder for media content |
| topics | Contains the actual content of the website, organized by subtopics |
| _site | This is the website generated from the rest of the content when `jekyll serve` is called |
| .jekyll-cache | Cache files for jekyll |

### _includes

This files in this folder describe the header, sidebar and tab-title elements of the webpage. 
They are all referenced by the page layout files in the _layouts folder (e.g. [default.html](../master/_layouts/default.html)).  

- **header.html** : The header is the top navigation bar and consists of the webpage title, tagline, home button and the topics dropdown. The entries in the dropdown are populated from the `collections` parameter in [_config.yml](../master/_config.yml). This file also contains scripts to handle toggling of the dropdown menu and making it disappear.

- **sidebar.html** : The sidebar is described here. It is a collection of links to the homepage and topics pages. The topics pages are dynamically populated from the `collections` parameter in [_config.yml](../master/_config.yml).

- **tab_title.html** : The text and icon that appear in the browser tab are specified here.

### _layouts
This folder contains several page layouts. 
Whenever a new page is created, it should use one of the layouts present in this folder. 
The layouts available are the following.

- **default.html** : This is the default page layout. This layout includes the tab_title, header, content and a footer. Sidebar is currently disabled, but can be added. 

- **page.html** : This layout extends the default layout and adds a page title to it. Webpages created with this layout are added to the sidebar.

- **post.html** : This layout extends the default layout and adds a title, date and links to related posts. Webpages created with this layout are not added to the sidebar.

### _sass

This folder contains .scss files that define the theme of the website.

- **_base**.scss : The base theme  
- **_code**.scss : For inline and block level code snippets  
- **_content**.scss : For content and footer  
- **_hyde**.scss : Global parameters of the Hyde theme  
- **_masthead**.scss : For the master header  
- **_message**.scss : For messages to users  
- **_pagination**.scss : For lightweight blog style pagination  
- **_posts**.scss : For posts and pages layouts  
- **_sidebar**.scss : For the sidebar  
- **_syntax**.scss : For code syntax highlighting  
- **_themes**.scss : Global parameters to select between different theme colours  
- **_topbar**.scss : For the top navigation bar  
- **_type**.scss : For all the typed text (typography)  
- **_variables**.scss : Global variables to tweak the theme  

### assets

The user places all the media into this folder.

### topics

This is the folder where the website's content is actually present.
The content is organized into folders and should have the following rules.

- Each folder should have a file called **index.md** which will be the landing page for that particular topic and contains a table of contents for all pages of that topic.
    - The header for the index.html should look like the following.
    ```
    ---
    title: Title of the page. This will appear in the dropdown.
    layout: page
    name: index
    ---
    ```
    - The rest of the page should have the following script that will auto populate the table of contents.  
    ```
    ## Contents

    {% for coll in site.collections %}
    {% if page.url contains coll.label %}

    {% for file in site[coll.label] %}
    {% if file.name contains "index" %}
    	{% continue %}
    {% endif %}

    - [{{ file.title | capitalize }}]({{site.baseurl|append:file.url}})

    {% endfor %}

    {% endif %}
    {% endfor %}
    ```  
    For each page present in the `collections`, the script looks for those that have the same label as that of the current index.md page. If that page is itself not an **index** page, then it adds a link to that page.

- Pages that describe the content should have the following header.
    ```
    ---
    title: Name of the file
    layout: page
    ---
    ```