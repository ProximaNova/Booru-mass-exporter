# Booru-mass-exporter

## Progress

This currently totally works at *.booru.org websites and partially works at other Danbooru-type websites.

## How-to

### Without tags

If there are no tags then you can add tags to the filenames. Example:
* Most images at http://steelvaginas.booru.org should have these tags:
 * `steel_vaginas piercing puffy tattoo vagina female solo light_skin`

To get the file list edit this HTML:

````html
<p id="xx">Save the text in here to a text file to download with "wget -i in.txt -O out.txt"</p>

<script>
text = "";
for (i=1; i<#+1; i++) {    // # = the most recent post
    text += "http://*.booru.org/index.php?page=post&s=view&id=" + i + "<br>"    // * = the hostname
}
document.getElementById("xx").innerHTML = text;

// Suffix all files with ".txt"
// then combine files:
// copy /b *.txt newfile.txt
</script>
````

* Use vim command: `g!/" id="image" /d | %s/^.*src="//g | %s/" id="image".*$//g`
* then save that to a text file
* then use: `wget -i in.txt`

### With tags

````html
<p id="xx">Save the text in here to a text file to download with "wget -i in.txt -O out.txt"</p>

<script>
text = "";
for (i=1; i<#+1; i++) {
    text += "http://*.booru.org/index.php?page=post&s=view&id=" + i + "<br>"
}
document.getElementById("xx").innerHTML = text;
</script>
````

Then run these Vim commands:

`%s/<.DOCTYPE html PUBLIC\(.*\n\)\{61}.*<.script><.span><span class="thumb"\(.*\n\)\{127}<\/body><\/html>//ge | %s/..DOCTYPE html PUBLIC\(.*\n\)\{48}//ge | %s/          Score: \(.*\n\)\{9}.*alt="img" src="//ge | %s/" id="image"\(.*\n\)\{14}/\r/ge | %s/.*>Next Post<br \(.*\n\)\{7}.*cols="40" rows="5"//ge | %s/<\/textarea>.\n.*My Tags\(.*\n\)\{134}//ge | %s/\(<\/div>\)\{4}.\n<\/body><\/html>/\r\r/ge | %s/.*\n.* name="parent" value="" .>.\n//ge | %s/.* id="title" value="\(Booru mass uploader\)\?" .>.\n//ge | %s/.* Source: \(http:..ibsearch.*upload\|https:..ibsearch\.xxx\)\? <br .>.\n//ge | %s/ga.src = ('https:' == document\(.*\n\)\{6}//ge | %s/.*Rating: \(.\).*\n\(.*\)\n>/\2\r>\1 /ge | %s/ \+$//ge | %s/\n\{2,3}/\r/ge | %s/^http/wget http/ge | %s/\(\.....\?\)\n^>\(.*\)/\1 -O "\2 IgnoreTheLastTag\1"/ge`

`let counter=0|g//let counter=counter+1|s/IgnoreTheLastTag/\=counter`

(These operations could be better.)

Manuel work:
Then copy (find with ctrl-f):
* Fix stuff that is not uniform
* "Title..." (should be replaced to end of tags)
* "Parent..."
* "Source..."
(Use this to get rid of abnormalities: `g!/^wget/d`)

Then save the text to a .bat file and run it.

### With lots of images

JavaScript's for loop can only generate so much text before crashing; thus, Java should be used:

````
public class MassDownloadHuge
{
    public static void main(String[] args)
    {
       // max = 621851
       for (int i = 1; i < 621851 + 1; i++)
       {
          System.out.println("wget -U \"Mozilla\" http://.../post/show/" + i);
//                                              Replace "..." with the domain name.
       }
    }
}
````

Add Java to the "path" environment variable:

`>SET PATH=%PATH%;c:\...\javapath`

Output of "path" command:

`>path`
`PATH=C:\ProgramData\Oracle\Java\javapath;C:\Windows\system32;C:\Windows;...`

Generate the batch file:

`>java MassDownloadHuge > 0.bat`
