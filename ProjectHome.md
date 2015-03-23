This is the nroff-filtered output of mango on itself to double as a "screenshot" and documentation. Forgive the syntax highlighting: there is no way to deactivate and keep the whitespace and monospaced font properly.

```
mango(1)                         User Commands                        mango(1)



NAME
       mango  - Create a man page from a Go package's source file's documenta-
       tion.

SYNOPSIS
       mango [-help] [-import import_path]  [-name  name]  [-version  version]
       [-manual  manual] [-package package_name] [-section Sections] [-include
       Includes] [package-directory|package-files]

DESCRIPTION
       Mango generates man files from the source of  a  Go  package.   If  the
       package  is  main, it creates a section 1 man page.  Otherwise, it cre-
       ates a section 3 man page.

       Mango is a tool similar to godoc(1) and uses the same  conventions  and
       information so that documentation will look the same in both.  Further,
       Mango uses no special markup.  The text is formatted by  simple  rules,
       described  in  FORMATTING  below.  The input is the comments and AST of
       the Go package.  The output is raw troff markup (compatiable with nroff
       and groff) dumped to stdout for pipelining, see EXAMPLES.

       For  section 1 man pages, Mango bases the OPTIONS section on the use of
       flag(3) and a special comment.  It takes

              var file = flag.String("source", "", "Set source file")
              var toggle = flag.Bool("t", false, "Toggle mode")
              var banner = flag.Bool("title", "Hello", "Set banner")

       and produces:

              -source file
              -toggle
              -banner = Hello

       Extraction of this information will not work if  the  flag  package  is
       renamed  on import and it can only extract flags defined with flag.Type
       and not those defined with flag.TypeVar in the init() function.   Addi-
       tional information may be given with a comment like

              //Usage: %name %flags [optional-arg] required-arg

       The "Usage:" part is mandatory.  %name and %flags (or %flag) exist only
       to make the intent clear to a reader unfamiliar  with  mango(1).   Both
       may  be  used  together,  each  may be used separately, and neither are
       required.

OPTIONS
       -help  Display help

       -import import_path
              Specify import path

       -name name
              Specify name of man page

       -version version
              Specify version

       -manual manual
              Specify the manual: see man-pages(7)

       -package package_name
              Select package to use if there are multiple packages in a direc-
              tory

       -section Sections
              Generate  sections  from  a  comma-seperated  list of filenames.
              Each section will be named after the file name that contains  it
              (_ will be replaced by a space).  The contents of each file will
              be formatted by the same rules as if they  were  extracted  from
              comments.   To  include preformatted sections see -include.  You
              cannot override OPTIONS, BUGS or SEE ALSO.

       -include Includes
              Generate sections from  a  comma-seperated  list  of  filenames.
              Each  section will be named after the file name that contains it
              (_ will be replaced by a space).  The contents of each file will
              be included as-is.  To let mango do the formatting use -section.

EXAMPLES
       Format package in current directory with nroff:

              mango | nroff -man > name.section


       Format package in current directory as a ps file with groff:

              mango | groff -man > name.section.ps


       Format package in current directory as a pdf:

              mango | groff -man | ps2pdf - name.section.pdf


       Format ogle(3), and ogle(1), which exist in the same directory:

              mango $GOROOT/src/pkg/exp/ogle | nroff -man > ogle.3
              mango -package main  $GOROOT/src/pkg/exp/ogle  |  nroff  -man  >
              ogle.1


       Format your package in a makefile:

              mango -import $TARG $GOFILES | nroff -man > name.section


FORMATTING
       An  all caps word on a line by itself, followed and preceded by a blank
       like denotes a new section.

       Two consecutive newlines start a new paragraph.

       Indented paragraphs, with one tab or four spaces, maintain their  rela-
       tive  indentation  (a  uniform, initial indent is ignored) and no addi-
       tional formatting is applied

       Words beginning with a hyphen are assumed to be command line  switches,
       and they, including the hypen, are bolded.  Words ending with (.) where
       . is a valid (even if obscure) man page section are formatted with  the
       word  but  not the (.) in bold.  In addition, each word is added to the
       SEE ALSO section.

HEURISTICS
       If no directory or files are specified, Mango uses the current  working
       directory.   If there are multiple packages in the input directory, any
       package named 'documentation' is used as the package  level  documenta-
       tion.  If there is no 'documentation' package or there are still multi-
       ple packages after taking 'documentation' out of play, Mango  will  use
       the  package  with  the  same  name as the input directory.  If this is
       incorrect, or you wish to generate man pages for multiple  packages  in
       one  directory you can select them individually with the -package flag.

       The name of the man page is the name of the package for section  3  man
       pages  and the name of the file that contains func main() for section 1
       man pages.  This can be overridden with the -name flag,  but  only  for
       section 1 pages.

       For  man  3 pages, the import path defaults to the name of the package.
       It can be overridden with the -import flag and the

       If the -version flag is not used, Mango searches the AST for a const or
       var  declaration  named Version.  Failing that, it uses today's date as
       the version.

       If the -manual flag is not used, it defaults to "User Commands" for man
       1 pages and to "Go Packages" for man 3 pages, respectively.

       Sections  in  the  comments,  or specified by the -include or -sections
       flags, are in the same order as they appear in the comments,  with  the
       exception  that  DIAGNOSTICS,  ENVIRONMENT,  and  FILES  section appear
       first, and in that order; and that HISTORY appears after the  SEE  ALSO
       section.

       In  man  1  pages, the user-defined sections appear between the OPTIONS
       and BUGS sections.  In man 3 pages, they appear after  the  DESCRIPTION
       section.

BUGS
       Could print var or const before realizing none are exported.

       Does not render RHS of consts or vars for section 3.

       Need to special case flag.Var to make it format properly.

       No way to group short/long name option pairs.

SEE ALSO
       flag(3), godoc(1), man-pages(7)



version 2010-10-16                2010-10-16                          mango(1)
```