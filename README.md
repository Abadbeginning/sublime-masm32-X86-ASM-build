# Welcome to MWeb


```{
    "cmd": ["D:\\masm32\\bin\\asm.bat", "$file_base_name"],
    "file_regex": "^[ ]*File \"(...*?)\", line ([0-9]*)",
    "selector": "source.asm",
    "encoding":"cp936",
    "variants":
     [
          {
              "name": "Run",
              "cmd": ["cmd", "/k", "D:\\masm32\\bin\\asm.bat", "$file_base_name",
              "&&", "D:\\Program Files (x86)\\DOSBox-0.74\\DOSBox.exe"],
              "shell":true
          }
     ]
}

```What              is MWeb? MWeb is a Pro Markdown writing, note taking and static blog generator App. MWeb used Github Flavored Markdown syntax. Please check the MWeb official website: <http://www.mweb.im> introduction video, it will show you how to use MWeb. We also suggest that you check the MWeb official help document: <http://www.mweb.im/help.html>

## Things you should know

MWeb has two Mode, External Mode and Library Mode. 
In External Mode, you can edit classic text and markdown files from anywhere on your Mac. As an example, you can point MWeb to a folder on Dropbox. Library Mode design for note taking and static blog/website generator. For more info, please check the [MWeb official website](http://www.mweb.im) introduction video and help document.

## Help us to make MWeb better!

1. Tell your friends about MWeb.
2. Send a feedback: <coderforart+233@gmail.com>
3. Leave a review or at least a rating in Mac App Store.


