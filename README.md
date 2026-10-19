## What is this?

This is a fork to Nianeyna's [Archive of Our Own](https://archiveofourown.org/) fanfiction downloading program [ao3downloader](https://github.com/nianeyna/ao3downloader). The fork creates a command line argument style interface for using ao3downloader and is a archive of how I personally use ao3downloader.

## WARNINGS

PLEASE GO READ the ao3downloader README first before using this extension.

Beta software: may contain bugs or unintended behavior. Use at your own risk. Feedback and bug reports are greatly appreciated.


## Quick Start

I recommend cloning this repo and testing with that so nothing in your existing ao3downloader directory breaks.
`git clone https://github.com/kyrielie/ao3downloadercli`

I personally modified my existing ao3downloader directory by adding `ao3downloader/ao3downloader/cli/cli/cli.py` and replacing `pyproject.toml`.

Note to self: just add the one line to the pyproject.toml and `pipx install -e . --force`.

Use `python cli.py` to download fics. (full or relative file path)

Or install using pip:
```
cd ao3downloadercli
pip install -e .
```
Run ao3downloadercli to download files.


I don't like my files going relative to which directory I'm currently in, so I make a custom shell script to run in the same directory I used to run `ao3downloader.py` before you could PyPi install.

Use the full path output of `which python` and the full path of `cli.py` in ao3downloadercli.sh and add `ao3downloadercli` to path somehow— I usually drop a symlink where my path already points for homebrew. Is this the proper way to do things? Idk it works...

# How to Use
```
usage: cli.py [-h] [--format {epub,pdf,azw3,mobi,html}] [--no-login] urlortextfile [pages]

positional arguments:
  urlortextfile         Expects one of the following:
                        1. AO3 search / paged link
                        2. AO3 work / series link
                        3. path to text file containing one search/paged link (for long links)
                        4. path to text file containing list of work/series links
  pages                 Number of pages to download. (default=1)

options:
  -h, --help            show this help message and exit
  --format {epub,pdf,azw3,mobi,html}, -f {epub,pdf,azw3,mobi,html}
                        Choose format to download. (default=EPUB)
  --no-login            Disable login (default: enabled)
  ```

Make sure you wrap urls in " quote marks `"https://archiveofourown.org/works..."` to avoid errors.

For some reason it was easiest for me to write code that expects multiple formats in the form: `-f epub -f html -f mobi` etc. But also it's not working right now idk why :sob:, so only one format at a time.

So like you could also put multiple search / paged links interspaced with work links in your text file and number of pages you want to download from each, but why would you do that. I do not recommend doing that.

Still not sure how to start from last page stopped on with this. I think if your link contains the `&page=#` at the end that is the page that you will start downloading at.

## Helpful Tips

Firefox/Chrome
- Use extensions to get links fics you have read from your history or bookmarks.
- Instead of bookmarking use OneTab, it's easier to deal with.
- When using Tumblr fic libraries use Link Gopher to get the links. Suggestions and collections by other users are the best way to reap fics! 

Safari on ios
- Hold press the obscenely large number of tabs you have open, tap `Copy links` then paste in notes or other app and get the list of hyperlinks into your text editor. Notes -> airdrop to mac -> Obsidian -> text file works well enough.

Use regex + `sort -u` to format links.

Learn how to use vim to quickly create files. I haven't lol. 

I don't keep fics in the download folder, I use Calibre to manage them. Well, fail to manage them. But that means to avoid redundant downloads I use.

```
cat links.txt ignorelist.txt > temporary.txt
sort -u temporary.txt > ignorelist.txt
```

- When downloading missing fics from series, if you are logged in, and the downloader finds a link to a series that is inaccessible because you do not have permission to access the series page, the downloader will download all of the works linked on your user dashboard page, instead. Yes... really.
- Links containing more than 4095 characters may cause issues on Mac and Linux. To work around this (on Mac and Linux only!) enter `stty -icanon` into your terminal before running ao3downloader. When you are finished running ao3downloader, enter `stty icanon` to restore the default behavior. H/t github user verotheelf for this workaround.
- Links containing more than 8191 characters will cause problems on Windows. There is no workaround, other than using a different link. Thankfully, it is unlikely you will run into this problem, as 8191 characters is quite a lot.

Sometimes the join is two links on the same line that you have to go in and fix.


### AO3 Searching

So the reason I wrote this originally is not because I am a habitual data hoarder. I am that, but also before I discovered a way to get an OR search to work on AO3 I required massive links that excluded so much stuff I had already read or didn't want to read.

Those search links were so incredibly massive that they maxed out my browser url limit (2000 characters) but before that point they maxed out my terminal paste limit. There may be a way to fix `input()` like that but I wanted to stop having to tell ao3downloader my preferences each time anyways and just type out one command.

I later found that you can simply use and OR operator in the [Search Within Results](https://archiveofourown.org/admin_posts/329) field. You can also use [this cheatsheet](https://archiveofourown.org/admin_posts/10851). I find the operator `-creators: username to filter out all works by a particular creator` to filter out specific authors useful.

These are ids for the fandoms I'm in:
`fandom_ids:143679061 OR fandom_ids:2692 OR fandom_ids:5450 OR fandom_ids:6943 OR fandom_ids:101375 OR fandom_ids:136512 OR fandom_ids:232768 OR fandom_ids:236208 OR fandom_ids:258526 OR fandom_ids:524391 OR fandom_ids:658827 OR fandom_ids:969647 OR fandom_ids:1147379 OR fandom_ids:8774227 OR fandom_ids:10104017 OR fandom_ids:17959080 OR fandom_ids:18245130 OR fandom_ids:27251507 OR fandom_ids:42817852 OR fandom_ids:74497137 OR fandom_ids:109615657 OR fandom_ids:124953688 OR fandom_ids:541437 OR fandom_ids:190901`

To get the fandom id tag, you can use the checkboxes in the exclude section and keep excluding everything while sorting in a popular tag, then unchecking everything you don't want to get a url contianing all the (encoded) tags you want. Then just decode that url and format the ids how you need.

My previous scripts were named epub.py and sendlink.py, very descriptive and very intuitive. Epub did text files with links and sendlink dealt with links nad text files with links. Or something. I really forgot, which is why I needed to rewrite them and properly document everything. Luckily I remembered enough that I was able to merge the two together.

### More Tips

> [!NOTE]
> TIPS FROM THE ORIGINAL AO3DOWNLOADER:

- **IMPORTANT**: some of your input choices are saved in a file called <!--CHECK-->data.json<!--SETTINGS_FILE_NAME-->. In some cases you will not be able to change these choices unless you clear your settings by deleting <!--CHECK-->data.json<!--SETTINGS_FILE_NAME--> (or editing it, if you are comfortable with json). In addition, please note that saved settings include passwords and keys and are saved in plain text. **Use appropriate caution with this file.**
- **You may change certain behaviors of the script** by editing the file <!--CHECK-->settings.ini<!--INI_FILE_NAME-->. Some of the current configurable options are:
  - Whether the script should save your password - if set to 'false', you will need to re-enter your password every time you log in via the script. (Defaults to false.)
  - How many seconds to pause between requests to Ao3 - the default is 0 seconds, which means that pauses will only be initiated when Ao3 requests them. Normally you should not need to adjust this, but it can be useful if you are running into odd behavior related to the rate limit.
  - The file naming pattern to use. For most people ao3downloader's default file names should work fine, but if you don't like them, you can change that here.
  - Where downloads are saved. By default this is a folder called '<!--CHECK-->downloads<!--DOWNLOAD_FOLDER_NAME-->' inside the folder you started the script from, but you can change it to any folder on your computer using a relative (to the folder you started the script from) or absolute path.
- **The purpose of entering your ao3 login information** is to download archive-locked works or anything else that is not visible when you are not logged in. If you don't care about that, there is no need to enter your login information.
- **Ao3 limits the number of requests** a single user can make to the site in a given time period. When this limit is reached, the script will pause for the amount of time (usually a few minutes) that Ao3 requests. When this happens, the start time, end time, and length of the pause in seconds will be printed to the console. If you try to access Ao3 from your browser during this period, you will see a "Retry later" message. Don't be alarmed by this - it's normal, and you aren't in trouble. Simply wait for the specified amount of time and then refresh the page. Other than during these required pauses, you can use Ao3 as normal while the script is running.
- **If you choose to '<!--CHECK-->get works from all encountered series links<!--AO3_PROMPT_SERIES-->'** then if the script encounters a work that is part of a series, it will also download the entire series that the work is a part of. This can _dramatically_ extend the amount of time the script takes to run. If you don't want this, choose 'n' when you get this prompt. (Series that you have bookmarked directly will always be fully downloaded, regardless of what you choose here.)
- **If you choose to '<!--CHECK-->download embedded images<!--AO3_PROMPT_IMAGES-->'** the script will look for image links on all works it downloads and attempt to save those images to an '<!--CHECK-->images<!--IMAGE_FOLDER_NAME-->' subfolder. Images will be titled with the name of the fic + 'imgxxx' to distinguish them.
  - Note that this feature does not encode any association between the downloaded images and the fic file aside from the file name.
  - Most file formats will include embedded image files anyway, regardless of whether you choose this option. I have confirmed this for PDF, EPUB, MOBI, and AZW3 file formats. (If you saw me contradict this in an earlier version of this readme... no you didn't)
  - Should an image download fail, the details of the failure will be logged in the log file with the message '<!--CHECK-->Problem getting image<!--ERROR_IMAGE-->' along with the work link and the image link. It's a good idea to check the log file for these messages, since you may still be able to download the image manually or track it down some other way.
- **If you need to stop a download in the middle,** you can just close the window. When you restart the script:
  - If you are using the option '<!--CHECK-->download from ao3 link<!--ACTION_DESCRIPTION_AO3-->', you will be given an option to restart the download from the page you left off on. The program will attempt to avoid re-downloading works that are already in the downloads folder.
  - If you are using the option '<!--CHECK-->download bookmarks from pinboard<!--ACTION_DESCRIPTION_PINBOARD-->' or '<!--CHECK-->re-download fics saved in one format in a different format<!--ACTION_DESCRIPTION_REDOWNLOAD-->', the list of fics to download will be retrieved as normal but will then be filtered to remove work links that meet the following conditions:
    - A record of a download attempt for that link is present in the log file AND
      - There is a fic with the same title already in the downloads folder OR
      - The download was marked as unsuccessful
  - If you are using the option '<!--CHECK-->download latest version of incomplete fics<!--ACTION_DESCRIPTION_UPDATE-->' or '<!--CHECK-->download missing fics from series<!--ACTION_DESCRIPTION_UPDATE_SERIES-->', just make sure to add any fics you don't want to download again to your library (that is, the folder you entered when prompted '<!--CHECK-->input path to folder containing files you want to check for updates<!--UPDATE_PROMPT_INPUT-->') and clean up any old versions before re-starting the download.
  - Most methods of avoiding repeat downloads rely on a file called <!--CHECK-->log.jsonl<!--LOG_FILE_NAME--> which is generated by the script. Make sure not to move, delete, or modify <!--CHECK-->log.jsonl<!--LOG_FILE_NAME--> if you want these features to work. (Using the option to generate the log visualization file is fine.)
- **When checking for incomplete fics,** the code makes certain assumptions about how fic files are formatted. I have tried to make this logic as flexible as possible, but there is still some possibility that not all incomplete fics will be properly identified by the updater, especially if the files are old (since ao3 may have made changes to how they format fics for download over time) or have been edited.
- **Custom work skins** are not preserved in downloaded files. I don't currently have a way around that, however, when a work is downloaded the log entry for the download will contain a column (called 'workskin') indicating whether the work had a custom skin or not, so you can at least know which fics are in danger of looking garbled.
- **The reason the installation instructions are on a separate site** is because I didn't want to have to explain how to download the install scripts from github. The install scripts themselves (as well as the complete source code for the instructions site) are hosted in this repository. You can find them at `site/public/install`.
- If you are able to create <!--CHECK-->logvisualization.html<!--VISUALIZATION_FILE_NAME--> (menu option 'v'), take a look through the logs to see if there are any helpful error messages.
- If the downloader is taking a very long time to run but not successfully downloading very many files, your specific IP address may have been blocked or throttled by cloudflare. *Sometimes* you can work around this problem by downloading from a different IP address. Some ways to change your IP address are:
  - If you have a vpn running, turn it completely off while using ao3downloader.
  - If you have a phone hotspot, try using that (as long as your data plan supports it - ebooks aren't huge files, but you might still want to check your plan limits first). For the phone hotspot trick, ensure the "wifi" option on the phone is disabled, otherwise it'll just pipe your normal internet connection through and not change your IP.
  - Transport your whole entire laptop to another location that has wifi (library, friend's house, etc) and try there.
  - Restart your router. This only has a small chance of actually resetting your IP, but it's easy to do and there IS a chance so hey.
  - You can also try petitioning your ISP, sometimes they'll reassign your IP address if you ask. This one involves paperwork and they might say no, though.
  - For the sake of completeness I'll mention that you can also try getting a vpn if you don't have one, since that will also change your IP address. However, turning a vpn on is a lot less likely to help you than turning a vpn off, because most vpns are on cloudflare's shitlist. Also, if you decide to get a vpn please do due diligence to ensure you don't pick a predatory one.

