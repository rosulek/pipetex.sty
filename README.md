# pipetex.sty

Send the contents of a LaTeX environment to any command. 
The result will be rendered as LaTeX source in the document.

You can use this to include some domain-specific language (DSL) inline with your TeX source code, provided you have a suitable command to compile your DSL to LaTeX.

| :exclamation:  Invoking external commands requires the `-shell-escape` command-line option to LaTeX   |
|------------------------------------------------------------|


### Usage

```
\documentclass{article}

\usepackage{pipetex}
\pipetexcommand{perl}

\begin{document}

  The contents of the \texttt{pipetex} environment will be
  passed as STDIN to whatever command you specified in
  \texttt{\textbackslash pipetexcommand}.
  The STDOUT output of the command will be rendered as \LaTeX
  at this point in the document:

  \begin{center}
    \begin{pipetex}
      printf '\framebox{%s}', $_ for 1 .. 20;
    \end{pipetex}
  \end{center}
  
  In these examples the contents of the environment are 
  sent to \texttt{perl}.  Here is another example:
 
  \begin{center}
    \begin{pipetex}
      my @fib = (0,1);
      for my $i (2 .. 18) {
        $fib[$i] = $fib[$i-1] + $fib[$i-2];
      }
      print join ", ", @fib;
    \end{pipetex}
  \end{center}
  
  You can also override the default command by 
  passing an optional argument to the environment,
  like this:
  
  \begin{center}
    \begin{pipetex}[sort]
      every
      good
      boy
      deserves
      fudge
    \end{pipetex}
  \end{center}
  
  When the command gives an error (as indicated by
  its return code), the error message is rendered
  instead:

  \begin{center}
    \begin{pipetex}
        $this = 'is';
        nonsensical "Perl"
    \end{pipetex}
  \end{center}
  
\end{document}

```

Result:

<img width="713" alt="image" src="https://user-images.githubusercontent.com/4512530/156871876-a73dad16-0092-48ef-822b-c5730edc9fc5.png">

### Other features

If you use the `-output-dir` option to latex, things will get messed up.
This package can only read from the output-dir, but commands are executed in the local directory.
To address this, use `\pipetexdir{path/to/output/dir}` in your document preamble.
Unfortunately this must be done manually, because I don't know of a way to read what the `-output-dir` value is.

# Acknowledgements

I have no idea what I'm doing. 
All I did was copy/adapt [python-sty](https://github.com/brotchie/python-sty) from James Brotchie, which is itself a continuation of the [python](https://www.ctan.org/pkg/python) package from Martin R. Ehmsen.
