# Introduction {#introduction}

*Star* is a high-level symbolic programming language oriented to the needs of large-scale high performance processing in modern parallel and distributed computing environments.

*Star* is a `functional-first' language -- in that functions and other programs are first class values. However, it is explicitly not a `pure' language: it has support for updatable variables and structures. However, its bias is definitely in favor of functional programming and in order to get the best value from programming in *Star*, such side-effecting features should be used sparingly.

*Star* is strongly, statically, typed. What this means is that all programs and all values have a single type that is determined by a combination of type inference and explicit type annotations.

While this definitely increases the initial burden of the programmer; we believe that correctness of programs is a long-term productivity gain -- especially for large programs developed by teams of programmers.

The type language is very rich; including polymorphic types, type constraints and higher-rank and higher kinded types. Furthermore, except in cases where higher-ranked types are required, type inference is used extensively to reduce the burden of `type bureaucracy' on programmers.

*Star* is extensible; there are many mechanisms designed to allow extensions to the language to be designed simply and effectively. Using such techniques can significantly ease the burden of writing applications.

## About this Reference
This reference is the language definition of the *Star* language. It is intended to be thorough and as precise as possible in the features discussed. However, where appropriate, we give simple examples as illustrative background to the specification itself.

### Syntax Rules
\index{grammar notation}
\index{bnf grammars}

Throughout this document we introduce many syntactic features of the language. We use a variant of traditional BNF grammars to do this. The meta-grammar can be described using itself; as shown in Figure~\vref{metaGrammar}.

\begin{figure}[htbp]
\begin{eqnarray*}
\emph{MetaGrammar}& \arrow& \emph{Production}\sequence{\,}\emph{Production}\\
\emph{Production} &\arrow& \emph{NonTerminal}\ \q{::=}\ \emph{Body}\\
\emph{Production} &\arrowplus&\emph{NonTerminal}\ \q{::+}\ \emph{Body}\\
\emph{Body} &\arrow& \emph{Quoted}\,\choice\,\emph{NonTerminal}\,\choice\,\emph{Choice}\, \choice\,\emph{Optional}\,\choice\,\emph{Sequence}\,\choice\,( \emph{Body} )\\
\emph{Quoted} &\arrow &\q{Characters}\\
\emph{NonTerminal} &\arrow&\emph{Identifier}\\
\emph{Choice}& \arrow &\emph{Body}\sequence{|}\emph{Body}\\
\emph{Optional} &\arrow &\q{[} \emph{Body} \q{]}\\
\emph{Sequence}&\arrow&\emph{Body}\sequence{\rm[\emph{op}]}\emph{Body}\ \choice\,\emph{Body}\sequence{\rm[\emph{op}]}\emph{Body}\plus
\end{eqnarray*}
\caption{Meta-Grammar Used in this Reference}\label{metaGrammar}
\end{figure}

Some grammar combinations are worth explaining as they occur quite frequently and may not be `standard' in BNF-style grammars. For example the rules for actions contain:
\begin{eqnarray*}
\emph{Action}& \arrow&\q{\{}\, \emph{Action}\sequence{;}\emph{Action}\, \q{\}}
\end{eqnarray*}
This grammar rule defines an \emph{Action} as a possibly empty sequence of \emph{Action}s separated by semi-colons and enclosed in braces -- i.e., the classic definition of a block. 

The rule:
\begin{eqnarray*}
\emph{Decimal}& \arrow&\emph{Digit}\sequence{}\emph{Digit}\plus
\end{eqnarray*}
denotes a definition in which there must be at least one occurrence of a \emph{Digit}; in this case there is also no separator between the \emph{Digit}s.

Occasionally, where a non-terminal is not conveniently captured in a single production, later sections will \emph{add} to the definition of the non-terminal. This is signaled with a \arrowplus{} production, as in:
\begin{eqnarray*}
\emph{Expression}&\arrowplus&\emph{ListLiteral}
\end{eqnarray*}
which signals that, in addition to previously defined expressions, a \emph{ListLiteral} is also an \emph{Expression}.

### Typographical Conventions
\index{typographical conventions}
Any text on a programming language often has a significant number of examples of programs and program fragments. In this reference, we show these using a \q{typewriter}-like font, often broken out in a display form:
\begin{alltt}
\ldots
P has type integer
\ldots
\end{alltt}
We use the \q{\ldots} ellipsis to explicitly indicate a fragment of a program that is embedded in a context.
\index{ellipsis}
\index{...}

Occasionally, we have to show a somewhat generic fragment of a program where you, the programmer, are expected to put your own text in. We highlight such areas using an \q{\emph{italicized typewriter}} font:
\begin{alltt}
(\emph{Args}) => \emph{Expr}
\end{alltt}
This kind of notation is intended to suggest that \q{\emph{Args}} and \q{\emph{Expr}} are a kind of \emph{meta-variable} which are intended to be replaced by specific text.

\begin{aside}
Some parts of the text require more careful reading, or represent comments about potential implications of the main text. These notes are highlighted the way this note is, with a \dbend{} symbol.\footnote{Notes which are not really part of the main exposition, but still represent nuggets of wisdom are relegated to footnotes.}
\end{aside}
\begin{aside}
\begin{aside}
Occasionally, there are areas where the programmer may accidentally `trip over' some feature of the language. It seems necessary to mark these with a double \dbend{}\dbend{} symbol.
\end{aside}
\end{aside}