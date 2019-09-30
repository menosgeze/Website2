# The life of a beginner - Still more Arch!

## My first lesson
In order to see a live-preview of a hugo website like this, 
navigate to the main or root folder of the Website, and run:
    
    hugo server --theme book

1. The command `hugo` manages the website. 
2. The command `server` will host a server in your own computer.
3. The option `--minify` makes the output html code shorter (not supported or not working on my pc currently).
4. The option `--theme book` is the theme choice-name of this website.
5. I open http://localhost:1313/ in my browser to see a live preview. What does it mean by live? it changes when I save a change and reload the web-browser. 
6. See [here](https://github.com/alex-shpak/hugo-book#configuration) for
   configuration for this particular theme: **book**

The list of links that will appear in the right menu are the sections of this main page, which is located in:

{{<highlight bash>}}
    ./content/_index.md
{{</highlight>}}

The list of links of the left menu are documents listed in:
{{<highlight bash>}}
    ./content/docs/
{{</highlight>}}



## Listing Partitions 
To see a tree of the partitions in your hard drives and solid state drives, even usb and other mounted devices, just type:
{{<highlight bash>}}
    lsblk
{{</highlight>}}

In particular, if there is a usb flash drive, it will list it, and if it is mounted, it will list the path to the mounting folder. 

## Watching protected content from Streaming Websites.
(Aug-10-2019) Since I use Arch-linux (new to it) and Chromium (also new to it) in my desktop pc,  I was not able to play  Netflix or Amazon. Luckily, I found that rather than installing desktop applications for each of them, I could install WideVine from the AUR.

{{<highlight bash>}}
   yay WideVine
{{</highlight>}}

Not only did I not have to set it up, it worked perfectly and I am now able to watch Netflix and Amazon movies again.

## Mounting Windows to Linux
While I was transitioning from Windows to Linux, I wanted to retrieve my Windows personal files. One suggestion from was to use the package **ntfs-3g** installed with:

 {{<highlight bash>}}
   sudo pacman -S ntfs-3g
{{</highlight>}}

or with:
    
 {{<highlight bash>}}
   yay ntfs-3g
{{</highlight>}}

because it is a core package. Then one can just mount as usual:

{{<highlight bash>}}
    mount /dev/Windows_partition /dev/home_partition
{{</highlight>}}

In the tutorial
[here](https://www.tecmint.com/how-do-i-access-or-mount-windows-ntfs-partition-in-linux/)
Ravi Saive suggests to use the option **-t ntfs-3g** which by putting it before the source Windows_partition specifies this type of filesystem. 

## Highlighting code in HUGO websites

(Aug-13-2019) A good practice is to follow conventions and to make things easily readable. For this the website manager HUGO has made markdowns tags (kind of html tags) to highlight code in a lot of languages. See [here](https://gohugo.io/content-management/syntax-highlighting/) where I was in particular interested in Postgres, Python and bash highlighting.

## Fast History Search in the Z-shell
Look at [here](https://unix.stackexchange.com/questions/97843/how-can-i-search-history-with-text-already-entered-at-the-prompt-in-zsh)

Since I am using kitty, rather than writing these commands in **~/.zshrc**, I will also write these commands in **~/.config/kitty/kitty.conf**. 




