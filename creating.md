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

- <strong>header.html</strong> : The header is the top navigation bar and consists of the webpage title, tagline, home button and the topics dropdown. The entries in the dropdown are populated from the `collections` parameter in [_config.yml](../master/_config.yml). This file also contains scripts to handle toggling of the dropdown menu and making it disappear.

- <strong>sidebar.html</strong> : The sidebar is described here. It is a collection of links to the homepage and topics pages. The topics pages are dynamically populated from the `collections` parameter in [_config.yml](../master/_config.yml).

- <strong>tab_title.html</strong> : The text and icon that appear in the browser tab are specified here.

### _layouts
