
# Codemeta + Pandoc Examples

In this repository you see three examples of using Pandoc, Pandoc templates and codemeta.json to create an [about page](about.md), a [CITATION.cff](CITATION.cff) and an [installer.sh](https://caltechlibrary.github.io/irdmtools/installer.sh) file.

## Generating a CITATION.cff

The codemeta.json file can be use to generate a CITATION.cff file using the Pandoc template [codemeta-cff.tmpl](codemeta-cff.tmpl).

~~~shell
    echo '' | pandoc -s --metadata title='Citation' \
                        --metadata-file codemeta.json \
                        --template codemeta-cff.tmpl \
                        >CITATION.cff
~~~

## Generating an about page

Like [CITATION.cff](CITATION.cff) we can generate a standard about page,
[about.md](about.md) from our codemeta.json using the Pandoc template [codemeta-about.tmpl](codemeta-about.tmpl).

~~~shell
    echo '' | pandoc -s --metadata title='About' \
                        --metadata-file codemeta.json \
                        --template codemeta-about.tmpl \
                        >about.md
~~~

## Generating an installer.sh

The nice thing about the Pandoc approach is you can also use it as
a shell script generator. Today (2023) many software tools[1] developers
use have installation instructions like

~~~shell
    curl https://caltechlibrary.github.io/datatools/installer.sh | sh
~~~

With a single POSIX shell installer script you easily install your
tools on macOS and Linux.

The codemeta.json file provides much of what is needed to generate the
installer script from [codemeta-installer.tmpl](codemeta-installer.tmpl).
There are three addition Pandoc metadata variables needed not explicitly found in the codemeta.json. The installer script expects to use a Zip file as an q<F5>0Additionally you need to name the zip archive of the content to be installed needs to be predictably named.

The z
To have Pandoc generate a workable installer script requires to parts.
First the name of your installation zip files need to be predictable. 

Secondly the codemeta-installer.tmpl file needs to know some metadata inorder for the generated script to be runable.

The [codemeta.json] file provides much of the information
needed to render our **installer.sh**. Our template, 
does make some specific assumptions

- You've installation files are in a zip archive
- There hosted under the GitHub in the releases directory for your repository
- The name of the zip file is in the form of `<PACKAGE>-v<VERSION>-<OS_NAME>-<ARCHITECTURE>.zip`
- The version number in the codemeta.json file does NOT have a leading 'v'
- curl is available on the machine where you want to install the software and it has a network connection

PACKAGE
: This would be the name of your Git repository, e.g. datatools

VERSION
: This would be the version number without the leading "v", e.g. "0.0.1"

OS_NAME
: would be "macOS", "Linux" or "Windows"

MACHINE-TYPE
: This would be what is reported by the command `uname -m`

Rendering the **installer.sh** file can be done with the following Pandoc command.

~~~
echo '' | pandoc -s --metadata title='Installer' \
            --metadata-file codemeta.json \
            --template installer.sh
~~~

If you host this installer script can then be uploaded to your website. The
curl command would be of the form

~~~
    curl <URL_TO_SITE>/installer.sh | sh
~~~


[1]: Examples Rust with [Rustup](https://rustup.rs/) and Haskell's [GHCup](https://www.haskell.org/ghcup/)

