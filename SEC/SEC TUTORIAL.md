<b class="APPLICATION">SEC** is a powerful event correlation engine written entirely
in [Perl](http://www.perl.org) that is capable of handling a wide
variety of event correlation tasks. This article presents a range of examples, from
simple to complex, using SEC for event correlation tasks.

</b></div>
</div>

* * *

<div class="TOC">
<dl>
<dt>**Table of Contents**</dt>

<dt>1 [Introduction](#INTRODUCTION)</dt>

<dt>2 [Getting Started](#GETTINGSTARTED)</dt>

<dt>3 [SEC In Depth](#SECINDEPTH)</dt>

<dt>4 [Using Contexts](#USINGCONTEXTS)</dt>

<dt>5 [Applications to Logfile
Monitoring](#APPLICATIONSTOLOGFILEMONITORING)</dt>

<dt>6 [Part One Conclusion](#PARTONECONCLUSION)</dt>
</dl>
</div>

<div class="SECT1">

* * *

# Introduction

SEC was written by Risto Vaarandi, a professional software developer from Estonia, and
is available from the [SEC Home
Page.](http://simple-evcorr.sourceforge.net) Risto has provided a [Manual Page,](http://simple-evcorr.sourceforge.net/man.html) a [FAQ](http://simple-evcorr.sourceforge.net/FAQ.html) and some [Examples](http://simple-evcorr.sourceforge.net/rulesets) that are helpful in developing advanced solutions. There is
also an active [Mailing List.](http://lists.sourceforge.net/mailman/listinfo/simple-evcorr-users)

SEC is a perl script which reads an input stream from a file or pipe and applies
pattern matching operations to the input looking for patterns specified by rules, found
in configuration files. SEC has several advanced features that make it ideal for a wide
variety of event correlation tasks such as log-file analysis, state machine operations,
logic analysis and more.

This article introduces basic SEC use and operations, using examples from simple to
complex. At the end of this article, the reader should have a solid working knowledge of
SEC and be able to create rules for event correlation operations.

<div class="NOTE">

> **Note:** <b class="APPLICATION">SEC** is copyrighted by Risto Vaarandi,
> (risto.vaarandi@eyp.ee) and is distributed under the terms of the GNU GPL. Work on SEC is
> supported by the [Union Bank of Estonia](http://www.eyp.ee).
> 
> </b>

</div>
</div>

<div class="SECT1">

* * *

# Getting Started

To get started with SEC, download the latest version from the [SEC
download page at Sourceforge.](http://sourceforge.net/project/showfiles.php?group_id=42089) Unpack the tarball, cd into the directory containing
the <b class="APPLICATION">sec.pl** perl script.

<div class="SECT2">

* * *

## 2.1 A Simple
Example

SEC uses a configuration file and takes input from a file or a named pipe. Perform the
following steps to create the first example:

*   

Create a new text file <tt class="FILENAME">C2.1.01.conf</tt> with your favorite
editor, and copy in the following text:

    
# Example C2.1.01.conf
    # Recognize a pattern and log it.
    #
    type=Single [![(1)](./imagelib/callouts/1.png)]()
    ptype=RegExp [![(2)](./imagelib/callouts/2.png)]()
    pattern=foo\s+(\S+) [![(3)](./imagelib/callouts/3.png)]()
    desc=$0 [![(4)](./imagelib/callouts/4.png)]()
    action=logonly[![(5)](./imagelib/callouts/5.png)]()

This example illustrates the following:


<dt>[![(1)](./imagelib/callouts/1.png)](#CO-A01-TYPE)</dt>

<dd><tt class="LITERAL">Single</tt> is the rule type. SEC includes several different
types of rules that are useful in event correlation. This rule is of type <span class="emphasis"><i class="EMPHASIS">Single*</i></span>.</dd>

<dt>[![(2)](./imagelib/callouts/2.png)](#CO-A01-PTYPE)</dt>

<dd><tt class="LITERAL">RegExp</tt> is the pattern type, one of <span class="emphasis"><i class="EMPHASIS">RegExp*</i></span> (for ``Regular Expression'') matching or <span class="emphasis"><i class="EMPHASIS">SubStr*</i></span>, for simpler string matching.</dd>

<dt>[![(3)](./imagelib/callouts/3.png)](#CO-A01-PATTERN)</dt>

<dd><tt class="LITERAL">foo\s+(\S+)</tt> is the actual pattern- in this case a perl
regular expression pattern. This pattern matches the word <tt class="LITERAL">foo</tt>
followed by one or more spaces, followed by one or more non-space characters, such as <tt class="LITERAL">bar, grok, or 1:443z--?.</tt> If you are new to perl regular expressions,
you are well advised to read Nicholas Clarks introduction [perlretut](http://search.cpan.org/~nwclark/perl-5.8.2/pod/perlretut.pod).</dd>

<dt>[![(4)](./imagelib/callouts/4.png)](#CO-A01-DESC)</dt>

<dd><tt class="LITERAL">desc</tt> is a variable definition for the pattern description.
In this case a perl numbered variable, <tt class="LITERAL">$0</tt>, is set to the entire
matched pattern.</dd>

<dt>[![(5)](./imagelib/callouts/5.png)](#CO-A01-ACTION)</dt>

<dd>The <tt class="LITERAL">action</tt> statement describes the action taken when the
pattern is recognized. In this case, the <tt class="LITERAL">logonly</tt> action simply
writes the pattern to the logfile if one is indicated on the command line, or to standard
output if not.</dd>
</dl>
</div>

*   

Save the file (<tt class="FILENAME">C2.1.01.conf</tt>) and execute the following
command:

    <tt class="PROMPT">%</tt> <tt class="USERINPUT">**perl sec.pl -conf=C2.1.01.conf -input=-**</tt>

This example will take input from directly from the terminal. Type the following lines
of input:

    <tt class="USERINPUT">**foo
    foo bar
    baz
    bar foo
    bar foo baz**</tt>

Notice that SEC responds by replying (logging to standard output) every time a pattern
is matched:

    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo bar**</tt>
    foo bar
    <tt class="USERINPUT">**baz**</tt>
    <tt class="USERINPUT">**bar foo**</tt>
    <tt class="USERINPUT">**bar foo baz**</tt>
    bar foo baz

In this example, you have created a SEC rule that matches a regular expression, and
tested it with input from the terminal.

*   

To see how SEC operates on files, instead of standard input, copy the test input above
into a temporary file, <tt class="FILENAME">T2.1.01.txt</tt>. Next, create an empty file
that you intend to monitor with SEC, <tt class="FILENAME">monitor.me</tt>.

Now execute SEC with the following command:

    <tt class="PROMPT">%</tt> <tt class="USERINPUT">**perl sec.pl -conf=C2.1.01.conf -input=monitor.me**</tt>

    ~/sec-2.1.11% perl sec.pl -input=monitor.me -conf=C2.1.01.conf
    Simple Event Correlator version 2.1.11
    Reading configuration from C2.1.01.conf
    1 rules loaded from C2.1.01.conf

SEC is now running in your terminal session, and reading input from <tt class="FILENAME">monitor.me</tt>. In a separate window, or terminal session in the same
directory, perform the following:

    <tt class="PROMPT">%</tt> <tt class="USERINPUT">**cat T2.1.01.txt >> monitor.me**</tt>

    ~/sec-2.1.11% perl sec.pl -input=monitor.me -conf=C2.1.01.conf
    Simple Event Correlator version 2.1.11
    Reading configuration from C2.1.01.conf
    1 rules loaded from C2.1.01.conf
    foo bar
    bar foo baz
    ^C

SEC parsed the input that arrived in <tt class="FILENAME">monitor.me</tt> and
performed it's actions (logging to standard output) when it recognized the patterns in
the input stream.

This is the basic operation of SEC. In this case, the ``events'' were the arrival of 2
matched pattern in the input stream. Although this is a simple example, SEC is capable of
far more powerful matching and complex operation.

</div>

<div class="SECT2">

* * *

## [2.2 Other SEC
Actions]()

SEC has well over a dozen different actions it can perform once it matches a pattern
in the input stream. Some of the actions depend on <span class="emphasis"><i class="EMPHASIS">contexts*</i></span> which will be described in a later section. The
following sections describe the actions that can be used all by themselves.

<div class="SECT3">

* * *

### [2.2.1 write]()

<tt class="LITERAL">write <filename> [<event text>]</tt>

This example uses the <span class="emphasis"><i class="EMPHASIS">write*</i></span>
action. The <span class="emphasis"><i class="EMPHASIS">write*</i></span> action writes the
specified text to the named <tt class="FILENAME">filename</tt>.

Copy

    # Example C2.2.1.01.conf
    # Recognize a pattern and write to a file.
    # Writes $0 Complete matched pattern, to file.
    #

    type=Single
    ptype=RegExp
    pattern=foo\s+(\S+)
    desc=$0
    action=write - Hello from SEC.  Matched text was $0

to <tt class="FILENAME">C2.2.1.01.conf</tt>. Note that the action statement now
reads:  

    action=write - Hello from SEC. Matched text was $0

A filename of ``-'' means write to standard output.

Save <tt class="FILENAME">C2.2.1.01.conf</tt> and run the example with input taken
from standard input:

    <tt class="PROMPT">%</tt> <tt class="USERINPUT">**perl sec.pl -conf=C2.2.1.01.conf -input=-**</tt>

Type the matched pattern:

    <tt class="USERINPUT">**foo baz**</tt>

and you should see the new action statement thus:  

    ~/SEC-examples%perl sec.pl -conf=C2.2.1.01.conf -input=-
    Simple Event Correlator version 2.1.11
    Reading configuration from C2.2.1.01.conf
    1 rules loaded from C2.2.1.01.conf
    <tt class="USERINPUT">**foo baz**</tt>
    Writing event 'Hello from SEC.  Matched text was foo baz' to file -
    Hello from SEC.  Matched text was foo baz

The line

    Writing event 'Hello from SEC.  Matched text was foo baz' to file -

is an example of SEC informative debug output. To prevent SEC from writing it's
informative output, change the debug level with the addition of the <span class="emphasis"><i class="EMPHASIS">-debug=n*</i></span> command line parameter: 

    <tt class="PROMPT">%</tt> <tt class="USERINPUT">**perl sec.pl -conf=C2.2.1.01.conf -input=- -debug=4**</tt>

A level of <tt class="OPTION">-debug=4</tt> outputs only notification messages and lower
levels of debug output as in:

    ~/SEC-examples%perl sec.pl -conf=C2.2.1.01.conf -input=- -debug=4
    Simple Event Correlator version 2.1.11
    Reading configuration from C2.2.1.01.conf
    <tt class="USERINPUT">**foo baz**</tt>
    Hello from SEC.  Matched text was foo baz
    ^C

</div>

<div class="SECT3">

* * *

### [2.2.2 shellcmd]()

<tt class="LITERAL">shellcmd <shellcmd></tt>

The <span class="emphasis"><i class="EMPHASIS">shellcmd*</i></span> action causes SEC
to execute a shell command. The shell command can be any executable program permitted by
normal user privileges.

Copy the following small shell script into <tt class="FILENAME">S2.2.2.01.sh</tt>.

    #!/bin/sh
    # Shell command for Example C2.2.2.01.conf
    #
    echo "Start of S2.2.2.01.sh  shell script"
    echo "Finding number of files in directory..."
    ls | wc -l
    echo "End of shell script"

Now copy the following SEC configuration into <tt class="FILENAME">C2.2.2.01.conf</tt>.

    # Example C2.2.2.01.conf
    # Recognize a pattern and run a shell script.
    #

    type=Single
    ptype=RegExp
    pattern=foo\s+(\S+)
    desc=$0
    action=shellcmd S2.2.2.01.sh

Run with:

    <tt class="PROMPT">%</tt> <tt class="USERINPUT">**perl sec.pl -conf=C2.2.2.01.conf -input=-**</tt>

If the shell command is not executable at all, SEC will indicate with the warning:

    Rule in C2.2.2.01.conf at line 5: Warning - '/home/jpb/SEC-examples/S2.2.2.01.sh' is not executable

If other errors prevent operation of the command, SEC will indicate with suitable warning
or error messages. Otherwise, when SEC matches the pattern in the input stream, it will
execute the script as requested:

    jpb@jpb-lt:~/SEC-examples$perl sec.pl -conf=C2.2.2.01.conf -input=-
    Simple Event Correlator version 2.1.11
    Reading configuration from C2.2.2.01.conf
    1 rules loaded from C2.2.2.01.conf
    <tt class="USERINPUT">**foo bar**</tt>
    Executing shell command '/home/jpb/SEC-examples/S2.2.2.01.sh'
    Child 14809 created for command '/home/jpb/SEC-examples/S2.2.2.01.sh'
    Start of S2.2.2.01.sh  shell script
    Finding number of files in directory...
          33
    End of shell script
    ^C

Control of the extra debug output is similar to that described above.   

<div class="NOTE">

> **Note:** User programs or scripts that are run with the <span class="emphasis"><i class="EMPHASIS">shell*</i></span> or <span class="emphasis"><i class="EMPHASIS">spawn*</i></span> (see below) actions must be in the PATH available to
> SEC at run time and not just in the current directory where SEC was started. In
> particular, if SEC is started with the <span class="emphasis"><i class="EMPHASIS">-detach*</i></span> option (described in Section 3) to make it a daemon
> process, the working directory for SEC is changed to the root directory.
> 
> Use of fully qualified path names for scripts is recommended.

</div>
</div>

<div class="SECT3">

* * *

### [2.2.3 spawn]()

<tt class="LITERAL">spawn <shellcmd></tt>

The <span class="emphasis"><i class="EMPHASIS">spawn*</i></span> action is identical to
the <span class="emphasis"><i class="EMPHASIS">shellcmd*</i></span> action, except that
output from the command is fed back into SEC for pattern matching.

Copy the following into <tt class="FILENAME">C2.2.3.01.conf</tt>:

    # Example C2.2.3.01.conf
    # Recognize a pattern and use the spawn action.
    #

    type=Single
    ptype=RegExp
    pattern=foo\s+(\S+)
    desc=$0
    action=spawn /home/jpb/SEC-examples/S2.2.3.01.sh

    # This rule will match part of the output of S2.2.3.01.sh
    type=Single
    ptype=RegExp
    pattern=ABCD_(.*)
    desc=$0
    action=write - Matched $0 from $1

Now copy the following to <tt class="FILENAME">S2.2.3.01.sh</tt>:

    #!/bin/sh
    # Shell command for Example C2.2.3.01.conf
    #
    echo "Start of S2.2.3.01.sh  shell command"
    echo "ABCD_"$$
    echo "End of shell script"

The <tt class="FILENAME">C2.2.3.01.conf</tt> rule file now contains two rules. The
first rule matches the same pattern as the previous examples, and will <span class="emphasis"><i class="EMPHASIS">spawn*</i></span> the shell script <tt class="FILENAME">S2.2.3.01.sh</tt> when triggered.

The second rule matches the pattern <tt class="LITERAL">ABC_(.*)</tt>, meaning match
<tt class="LITERAL">ABCD_</tt> followed by any string of characters until the end of the
line.

The <tt class="FILENAME">S2.2.3.01.sh</tt> script executed by the first rule echos a
start banner then executes the line

    echo "ABCD_"$$

echoing literal <tt class="LITERAL">ABCD_</tt> followed by the process ID of the shell
script.  

We will need the informational debug output, so use the command:

    <tt class="PROMPT">%</tt> <tt class="USERINPUT">**perl sec.pl -conf=C2.2.3.01.conf -input=-**</tt>

Assuming no errors, the output looks like this:

    jpb@jpb-lt:~/SEC-examples$perl sec.pl -conf=C2.2.3.01.conf -input=-
    Simple Event Correlator version 2.1.11
    Reading configuration from C2.2.3.01.conf
    2 rules loaded from C2.2.3.01.conf
    <tt class="USERINPUT">**foo bar**</tt>
    Spawning shell command '/home/jpb/SEC-examples/S2.2.3.01.sh'
    Child 14893 created for command '/home/jpb/SEC-examples/S2.2.3.01.sh'
    Creating event 'Start of S2.2.3.01.sh  shell command' (received from child 14893)
    Creating event 'ABCD_14893' (received from child 14893)
    Creating event 'End of shell script' (received from child 14893)
    Writing event 'Matched ABCD_14893 from 14893' to file -
    Matched ABCD_14893 from 14893
    ^C

The debug output shows how child process 14893 was created by SEC when the first
pattern was matched. The next three lines indicate ``<tt class="LITERAL">Creating event
...</tt>'' for each line of output by the <tt class="FILENAME">S2.2.3.01.sh</tt>
script.

The ``<tt class="LITERAL">Writing event ...</tt>'' line indicates that the second rule
has matched its pattern. Finally, the pattern itself is written to standard output by the
second rule action statement. This feedback mechanism can be very useful in many event
correlation tasks.

<span class="emphasis"><i class="EMPHASIS">spawn*</i></span> is quite often used to
create multiple input streams for SEC to process. See Section 3.3 for an example of this
usage.

</div>

<div class="SECT3">

* * *

### [2.2.4 assign And
eval]()

<tt class="LITERAL">assign %<letter> [<text>]</tt>

<tt class="LITERAL">eval %<letter> <code></tt>

Both <span class="emphasis"><i class="EMPHASIS">assign*</i></span> and <span class="emphasis"><i class="EMPHASIS">eval*</i></span> deal with ``% <letter>''
variables. These special variables are of the form ``<tt class="LITERAL">%n</tt>'' where
``<tt class="LITERAL">n</tt>'' is a single letter from the set ``<tt class="LITERAL">[A-Za-z]</tt>''. These variables are <span class="emphasis"><i class="EMPHASIS">not*</i></span> perl hashes. They are internal SEC variables that can be
used in rules. SEC reserves the following special variables:

<div class="INFORMALTABLE">
<table border="0" class="CALSTABLE">
<tbody>
<tr>
<td align="LEFT" valign="TOP"><span class="emphasis"><i class="EMPHASIS">Variable*</i></span></td>
<td align="LEFT" valign="TOP"><span class="emphasis"><i class="EMPHASIS">Description*</i></span></td>
</tr>

<tr>
<td align="LEFT" valign="TOP">%s</td>
<td align="LEFT" valign="TOP">The event description string- that is, the entire matched
pattern if the parameter is ``<tt class="LITERAL">$0</tt>''. Otherwise it is the entire
operand of the <span class="emphasis"><i class="EMPHASIS">desc*</i></span> parameter,
including backreference substitutions if any.</td>
</tr>

<tr>
<td align="LEFT" valign="TOP">%t</td>
<td align="LEFT" valign="TOP">Textual timestamp, as returned by date(1)</td>
</tr>

<tr>
<td align="LEFT" valign="TOP">%u</td>
<td align="LEFT" valign="TOP">Numeric timestamp, as returned by time(2)</td>
</tr>
</tbody>
</table>
</div>

Special variables have global scope across multiple SEC rule files. If an assignment
is made by either the <span class="emphasis"><i class="EMPHASIS">assign*</i></span> or
<span class="emphasis"><i class="EMPHASIS">eval*</i></span> actions, SEC maintains that
assignment for the life of the program or until the next assignment to that variable.

Copy the following to <tt class="FILENAME">C2.2.4.01a.conf</tt>:

    # Example C2.2.4.01a.conf
    # Recognize a pattern and use the assign and eval actions.
    #
    type=Single
    ptype=RegExp
    pattern=foo
    desc=$0
    action=assign %f Mary had a little lamb,

Copy the following to <tt class="FILENAME">C2.2.4.01b.conf</tt>:

    # Example C2.2.4.01b.conf
    # This rule uses the eval action to assign its variable.
    type=Single
    ptype=RegExp
    pattern=bar
    desc=$0
    action=eval %h ($t = "its fleece had neon glow")

Copy the following to <tt class="FILENAME">C2.2.4.01c.conf</tt>:

    # Example C2.2.4.01c.conf
    # This rule uses the write action to write %f, %h, and %t
    type=Single
    ptype=RegExp
    pattern=baz
    desc=$0
    action=write - %f %h at %t

Note that the patterns for this example have been simplified to match just the single
words ``<tt class="LITERAL">foo</tt>'' ``<tt class="LITERAL">bar</tt>'' and ``<tt class="LITERAL">baz</tt>''.

For this example, SEC must be told to use all three configuration files:

    <tt class="PROMPT">%</tt> <tt class="USERINPUT">**perl sec.pl -conf=C2.2.4.01a.conf \
          -conf=C2.2.4.01b.conf \
          -conf=C2.2.4.01c.conf \
          -input=-**</tt>

and input the three words in order:

    jpb@jpb-lt:~/SEC-examples$perl sec.pl -conf=C2.2.4.01a.conf \
    >           -conf=C2.2.4.01b.conf \
    >           -conf=C2.2.4.01c.conf \
    >           -input=-
    Simple Event Correlator version 2.1.11
    Reading configuration from C2.2.4.01a.conf
    1 rules loaded from C2.2.4.01a.conf
    Reading configuration from C2.2.4.01b.conf
    1 rules loaded from C2.2.4.01b.conf
    Reading configuration from C2.2.4.01c.conf
    1 rules loaded from C2.2.4.01c.conf
    <tt class="USERINPUT">**foo**</tt>
    Assigning value 'Mary had a little lamb,' to variable '%f'
    <tt class="USERINPUT">**bar**</tt>
    Evaluating code '$t = "its fleece had a neon glow"' and setting variable '%h'
    Assigning value 'its fleece had a neon glow' to variable '%h'
    <tt class="USERINPUT">**baz**</tt>
    Writing event 'Mary had a little lamb, its fleece had a neon glow at Thu Nov 20 04:34:32 2003' to file -
    Mary had a little lamb, its fleece had a neon glow at Thu Nov 20 04:34:32 2003
    ^C

Each variable assignment is shown by the debug output. The ``<tt class="LITERAL">%t</tt>'' variable is set automatically by SEC.

</div>

<div class="SECT3">

* * *

### [2.2.5 event]()

<tt class="LITERAL">event [<time>] [<event text>]</tt>

Finally, there is the <span class="emphasis"><i class="EMPHASIS">event*</i></span>
action. <span class="emphasis"><i class="EMPHASIS">event*</i></span> allows the insertion
of input to SEC from inside SEC itself. It is a simple feedback mechanism- one controlled
by SEC's own rules. The <span class="emphasis"><i class="EMPHASIS">time*</i></span>
parameter is the number of seconds to wait before inserting the event text into SEC's
input stream. For non-zero <span class="emphasis"><i class="EMPHASIS">time*</i></span>
values this is, in effect, asynchronous in that other input events can be processed
between the time the <span class="emphasis"><i class="EMPHASIS">event*</i></span> action
is called and the time the event text is recognized. Both cases are considered below.

Note that this example also introduces the capability to use multiple actions
(separated by a ``;'') within the same rule.

First, copy the following to <tt class="FILENAME">C2.2.5.01.conf</tt>:

    # Example C2.2.5.01.conf
    # Use the event action to queue input in the future.
    # Also an example of using multiple actions, and
    # $variable usage.
    #
    type=Single
    ptype=RegExp
    pattern=foo
    desc=$0
    action=event 5 baz is now matched. ; \
           write - foo matched at %t.  baz event in 5 seconds...

    # 
    type=Single
    ptype=RegExp
    pattern=bar
    desc=$0
    action=write - $0 is matched.

    # This rule will match the 'baz' event in rule 1
    type=Single
    ptype=RegExp
    pattern=baz
    desc=$0
    action=write - %s matched at %t

Run with:

    <tt class="PROMPT">%</tt> <tt class="USERINPUT">**perl sec.pl -conf=C2.2.5.01.conf -input=-**</tt>

Enter the first input pattern ``<tt class="LITERAL">foo</tt>'' and then immediately
enter the second input pattern ``<tt class="LITERAL">bar</tt>''. Then wait and observe
that SEC processes the input for ``<tt class="LITERAL">baz</tt>'' after the requested
delay of 5 seconds.

    jpb@jpb-lt:~/SEC-examples$perl sec.pl -conf=C2.2.5.01.conf -input=-
    Simple Event Correlator version 2.1.11
    Reading configuration from C2.2.5.01.conf
    3 rules loaded from C2.2.5.01.conf
    <tt class="USERINPUT">**foo**</tt>
    Writing event 'foo matched at Sat Nov 15 07:03:10 2003.  baz event in 5 seconds...' to file -
    foo matched at Sat Nov 15 07:03:10 2003.  baz event in 5 seconds...
    <tt class="USERINPUT">**bar**</tt>
    Writing event 'bar is matched.' to file -
    bar is matched.
    Creating event 'baz is now matched.'
    Writing event 'baz is now matched. matched at Sat Nov 15 07:03:15 2003' to file -
    baz is now matched. matched at Sat Nov 15 07:03:15 2003
    ^C

For a <span class="emphasis"><i class="EMPHASIS">time*</i></span> value of zero
however, SEC processes events immediately- even before the next line of input from the
input stream. To demonstrate this, change action lines in the first rule to read:

    action=event 0 baz is now matched.; \
           write - foo matched at %t.

and rerun the example.  

The output now becomes:

    jpb@jpb-lt:~/SEC-examples$perl sec.pl -conf=C2.2.5.01.conf -input=-
    Simple Event Correlator version 2.1.11
    Reading configuration from C2.2.5.01.conf
    3 rules loaded from C2.2.5.01.conf
    <tt class="USERINPUT">**foo**</tt>
    Creating event 'baz is now matched.'
    Writing event 'foo matched at Sat Nov 15 07:13:35 2003.' to file -
    foo matched at Sat Nov 15 07:13:35 2003.
    Writing event 'baz is now matched. matched at Sat Nov 15 07:13:35 2003' to file -
    baz is now matched. matched at Sat Nov 15 07:13:35 2003
    <tt class="USERINPUT">**bar**</tt>
    Writing event 'bar is matched.' to file -
    bar is matched.
    ^C

This ordering of <span class="emphasis"><i class="EMPHASIS">event*</i></span> is true
even when input is read from a fast input stream.

</div>
</div>
</b></div>

<div class="SECT1">

* * *

# [3 SEC In Depth]()

SEC has many parameters that control it's operation. These are viewed by simply
calling SEC with no parameters:

    <tt class="PROMPT">%</tt> <tt class="USERINPUT">**perl sec.pl**</tt>

    Version: 2.1.11

    Usage:
      sec.pl -input=<inputfile> -conf=<conffile pattern> ...

    Optional flags:
      -input_timeout=<input timeout>
      -timeout_script=<timeout script>
      -reopen_timeout=<reopen timeout>
      -poll_timeout=<poll timeout>
      -blocksize=<io block size>
      -log=<logfile>
      -debug=<debuglevel>
      -pid=<pidfile>
      -dump=<dumpfile>
      -cleantime=<clean time>
      -bufsize=<input buffer size>
      -evstoresize=<event store size>
      -quoting, -noquoting
      -tail, -notail
      -fromstart, -nofromstart
      -detach, -nodetach
      -intevents, -nointevents
      -testonly, -notestonly

    Obsolete flags:
      -separator=<separator>

All options are fully described in the SEC [Manual Page](http://simple-evcorr.sourceforge.net/man.html). Options
of the form <tt class="OPTION">-name=value</tt> are required to have a value. As noted
above the ``-conf=<conffile pattern>'' and ``-input=<inputfile>'' options are
required when executing <b class="APPLICATION">perl sec.pl**. A brief review of some of
the more common options follows:   

<div class="INFORMALTABLE">
<table border="0" class="CALSTABLE">
<tbody>
<tr>
<td align="LEFT" valign="TOP"><span class="emphasis"><i class="EMPHASIS">Option*</i></span></td>
<td align="LEFT" valign="TOP"><span class="emphasis"><i class="EMPHASIS">Description*</i></span></td>
</tr>

<tr>
<td align="LEFT" valign="TOP">-log=<logfile></td>
<td align="LEFT" valign="TOP">This option specifies the location of a logfile that SEC
uses to track it's operation, such as pattern matches, actions, etc. The volume of
information is controlled by the <tt class="OPTION">-debug</tt> option.</td>
</tr>

<tr>
<td align="LEFT" valign="TOP">-debug=<debuglevel></td>
<td align="LEFT" valign="TOP">This option controls how verbose SEC is as it tracks it's
operation. The values range between 1 (critical messages) and 6 (debug messages). Each
level includes output from lower levels.</td>
</tr>

<tr>
<td align="LEFT" valign="TOP">-pid=<pidfile></td>
<td align="LEFT" valign="TOP">This option provides for the location of a process ID file.
SEC will write it's process ID to this file upon startup.</td>
</tr>

<tr>
<td align="LEFT" valign="TOP">-dump=<dumpfile></td>
<td align="LEFT" valign="TOP">This option provides for the location of a dump file where
SEC can dump its internal data structures, variables and other information upon receipt
of the <span class="emphasis"><i class="EMPHASIS">USR1*</i></span> signal. The default
location is <tt class="FILENAME">/tmp/sec.dump</tt>.</td>
</tr>

<tr>
<td align="LEFT" valign="TOP">-detach</td>
<td align="LEFT" valign="TOP">Specifying this option causes SEC to detach itself from the
controlling terminal and run as a daemon process. The default is <tt class="OPTION">-nodetach</tt>.</td>
</tr>

<tr>
<td align="LEFT" valign="TOP">-intevents</td>
<td align="LEFT" valign="TOP">This option causes SEC to perform special processing at
startup. This special processing is described in the SEC man page.</td>
</tr>

<tr>
<td align="LEFT" valign="TOP">-testonly</td>
<td align="LEFT" valign="TOP">The ``-testonly'' option can be used to test for syntax
errors in configuration files. It does not start SEC for operation.</td>
</tr>
</tbody>
</table>
</div>

The above options are the most common in ordinary usage. See the [Manual Page](http://simple-evcorr.sourceforge.net/man.html) for more
information on these and other options.

<div class="SECT2">

* * *

## [3.1 SEC Rule Types]()

SEC contains several rule types (other than <span class="emphasis"><i class="EMPHASIS">Single*</i></span> introduced in Section 2) for event correlation. The
following table lists the other rule types and gives a short summary of each:

<div class="INFORMALTABLE">
<table border="0" class="CALSTABLE">
<tbody>
<tr>
<td align="LEFT" valign="TOP"><span class="emphasis"><i class="EMPHASIS">Rule
Type*</i></span></td>
<td align="LEFT" valign="TOP"><span class="emphasis"><i class="EMPHASIS">Description*</i></span></td>
</tr>

<tr>
<td align="LEFT" valign="TOP">SingleWithScript</td>
<td align="LEFT" valign="TOP">The <span class="emphasis"><i class="EMPHASIS">SingleWithScript*</i></span> rule integrates external scripts with SEC
pattern matching, not as an action as used in Section 2.2.2. Its rule definition is
similar to the Single rule, with the exception of an additional <span class="emphasis"><i class="EMPHASIS">script*</i></span> parameter.</td>
</tr>

<tr>
<td align="LEFT" valign="TOP">SingleWithSuppress</td>
<td align="LEFT" valign="TOP">The <span class="emphasis"><i class="EMPHASIS">SingleWithSuppress*</i></span> rule implements compression of multiple
matched events into a single event. Its rule definition is also similar to the Single
rule, with the exception of an additional <span class="emphasis"><i class="EMPHASIS">window*</i></span> parameter.</td>
</tr>

<tr>
<td align="LEFT" valign="TOP">Pair</td>
<td align="LEFT" valign="TOP">The <span class="emphasis"><i class="EMPHASIS">Pair*</i></span> combines two or more events into a single event within a
given time window. The rule definition for <span class="emphasis"><i class="EMPHASIS">Pair*</i></span> contains several new parameters.</td>
</tr>

<tr>
<td align="LEFT" valign="TOP">PairWithWindow</td>
<td align="LEFT" valign="TOP">The <span class="emphasis"><i class="EMPHASIS">PairWithWindow*</i></span> also correlates two or more events inside a
time window. The rule definition for <span class="emphasis"><i class="EMPHASIS">PairWithWindow*</i></span> is similar to the <span class="emphasis"><i class="EMPHASIS">Pair*</i></span>.</td>
</tr>

<tr>
<td align="LEFT" valign="TOP">SingleWithThreshold</td>
<td align="LEFT" valign="TOP">The <span class="emphasis"><i class="EMPHASIS">SingleWithThreshold*</i></span> rule counts matching events within a time
window up to a threshold. When the threshold is reached an action is executed and further
matching events after the threshold are ignored for the remainder of the time
window.</td>
</tr>

<tr>
<td align="LEFT" valign="TOP">SingleWith2Thresholds</td>
<td align="LEFT" valign="TOP">The <span class="emphasis"><i class="EMPHASIS">SingleWith2Thresholds*</i></span> rule counts matching events within time
window <span class="emphasis"><i class="EMPHASIS">t1*</i></span> and executes an action
when the the first threshold is reached. The count is reset and matched events are
counted for <span class="emphasis"><i class="EMPHASIS">t2*</i></span> seconds. Execute
another action if the count falls below the second threshold within time window <span class="emphasis"><i class="EMPHASIS">t2*</i></span>.</td>
</tr>

<tr>
<td align="LEFT" valign="TOP">Suppress</td>
<td align="LEFT" valign="TOP">The <span class="emphasis"><i class="EMPHASIS">Suppress*</i></span> rule suppresses matching input events. This rule has
no action, but simply filters out all matching events.</td>
</tr>

<tr>
<td align="LEFT" valign="TOP">Calendar</td>
<td align="LEFT" valign="TOP">The <span class="emphasis"><i class="EMPHASIS">Calendar*</i></span> rule executes an action at specific times. The time
parameters for this rule are similar to cron(8).</td>
</tr>
</tbody>
</table>
</div>
</div>

<div class="SECT2">

* * *

## [3.2 More SEC
Rule Examples]()

This section presents examples of the rules listed in Section 3.1. For the most part,
these rules do not depend on <span class="emphasis"><i class="EMPHASIS">contexts*</i></span> which will be covered in a later section.

Some of the rules will require input from different sessions at the same time. Keep
two or three sessions open to facilitate working with these rules.

<div class="SECT3">

* * *

### [3.2.1
SingleWithScript]()

The <span class="emphasis"><i class="EMPHASIS">SingleWithScript*</i></span> rule
combines matching a pattern and the execution of a separate program to determine if the
rule is matched. Running a separate program to validate or confirm whether an event is
valid is often necessary. For example, matching an IP address in a rule and checking
whether the IP address is on a list of valid addresses can not be done by pattern
matching in a rule alone. A separate program is required to determine if the matched IP
address is on the list.

Copy the following to C3.2.1.01.conf:

    # Example C3.2.1.01.conf
    # Single with script.  Pass matched IP address
    # to script for validation. If valid, execute
    # action 1; if not valid execute action2.
    #
    # Note: change script path (and possibly perl path)
    #       to match your system.

    type=SingleWithScript
    ptype=RegExp
    pattern=(\d+)\.(\d+)\.(\d+)\.(\d+)
    script=/usr/bin/perl /home/jpb/SEC-examples/S3.2.1.01.pl $0
    desc=$0
    action=write - IP address $0  matches.
    action2=write - IP address $0  does NOT match.

Note that while this RegExp pattern used will match an IP address, it will also match
expressions that are not real IP addresses, such as ``<tt class="LITERAL">9999.8888.7777.6666</tt>''.

Also note that this rule takes two action statements. SEC checks the return value of
the called program. If the program returns a zero value, the <span class="emphasis"><i class="EMPHASIS">action*</i></span> is executed, if non-zero <span class="emphasis"><i class="EMPHASIS">action2*</i></span> is executed.

Next, copy the following to script <tt class="FILENAME">S3.2.1.01.pl</tt>

    #!/usr/bin/perl
    #
    # Script S3.2.1.01.pl - check if IP argument
    # matches a short list of IP addresses.
    # Return zero on success, 1 on failure.

    @match_list = ( '1.2.3.4',
                    '2.3.4.5',
                    '3.4.5.6'
                  );

    $ip_addr = $ARGV[0] or die "No IP address passed on command line";

    foreach $ip (@match_list)
    {
      exit (0) if $ip_addr eq $ip;
    }

    exit 1;

Script <tt class="FILENAME">S3.2.1.01.pl</tt> accepts a single IP address on the
command line passed from the matched rule. If the address matches one of the IPs on its
small list of IP addresses, it returns zero, else it returns 1. If there is no IP address
at all, the script dies and returns a non-zero value.

Run with:

    <tt class="PROMPT">%</tt> <tt class="USERINPUT">**perl sec.pl -conf=C3.2.1.01.conf -input=-**</tt>

Output looks like this:

    jpb@jpb-lt:~/SEC-examples$perl sec.pl -conf=C3.2.1.01.pl -input=-
    Simple Event Correlator version 2.1.11
    Reading configuration from C3.2.1.01.pl
    Can't open configuration file C3.2.1.01.pl (No such file or directory)
    ^C
    jpb@jpb-lt:~/SEC-examples$perl sec.pl -conf=C3.2.1.01.conf -input=-
    Simple Event Correlator version 2.1.11
    Reading configuration from C3.2.1.01.conf
    1 rules loaded from C3.2.1.01.conf
    <tt class="USERINPUT">**1.2.3.4**</tt>
    Child 16396 created for command '/usr/bin/perl /home/jpb/SEC-examples/S3.2.1.01.pl 1.2.3.4'
    Child 16396 terminated with exitcode 0
    Writing event 'IP address 1.2.3.4  matches.' to file -
    IP address 1.2.3.4  matches.
    <tt class="USERINPUT">**5.6.7.8**</tt>
    Child 16398 created for command '/usr/bin/perl /home/jpb/SEC-examples/S3.2.1.01.pl 5.6.7.8'
    Child 16398 terminated with non-zero exitcode 1
    Writing event 'IP address 5.6.7.8  does NOT match.' to file -
    IP address 5.6.7.8  does NOT match.
    ^C

More robust IP address matching is possible with the <b class="APPLICATION">Net::IP_Addr** perl module.

</b></div>

<div class="SECT3">

* * *

### [3.2.2
SingleWithSuppress]()

With the <span class="emphasis"><i class="EMPHASIS">SingleWithSuppress*</i></span> rule
it is possible to become alerted to an event the first time it is seen, then ignore the
same event within a time window.

Copy the following to C3.2.2.01.conf:

    # Example C3.2.2.01.conf
    # Example of SingleWithSuppress
    #
    type=SingleWithSuppress
    ptype=RegExp
    pattern=foo
    desc=$0
    action=write - $0 suppressed for 5 seconds at %t
    window=5

Run with:

    <tt class="PROMPT">%</tt> <tt class="USERINPUT">**perl sec.pl -conf=C3.2.2.01.conf -input=-**</tt>

and continuously enter ``<tt class="LITERAL">foo</tt>'' as rapidly as possible.   

Output:

    jpb@jpb-lt:~/SEC-examples$perl sec.pl -conf=C3.2.2.01.conf -input=-
    Simple Event Correlator version 2.1.11
    Reading configuration from C3.2.2.01.conf
    1 rules loaded from C3.2.2.01.conf
    <tt class="USERINPUT">**foo**</tt>
    Writing event 'foo suppressed for 5 seconds at Sat Nov 15 17:04:38 2003' to file -
    foo suppressed for 5 seconds at Sat Nov 15 17:04:38 2003
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    Writing event 'foo suppressed for 5 seconds at Sat Nov 15 17:04:44 2003' to file -
    foo suppressed for 5 seconds at Sat Nov 15 17:04:44 2003
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    Writing event 'foo suppressed for 5 seconds at Sat Nov 15 17:04:50 2003' to file -
    foo suppressed for 5 seconds at Sat Nov 15 17:04:50 2003
    <tt class="USERINPUT">**foo**</tt>
    ^C

</div>

<div class="SECT3">

* * *

### [3.2.3 Pair]()

The <span class="emphasis"><i class="EMPHASIS">Pair*</i></span> rule handles two
different events, matched by two different patterns in its rule definition. The rule uses
a time window which is set upon the first occurrence of event A. If event B occurs within
the time window, events A and B are considered correlated, and the entire rule is
considered matched. Otherwise, the correlation operation for the pair terminates.

There are two action statements, each corresponding to its own pattern. Action one
(<span class="emphasis"><i class="EMPHASIS">action*</i></span>) is executed when event A
is matched. Action two (<span class="emphasis"><i class="EMPHASIS">action2*</i></span>) is
executed if event B occurs within the time window.

Copy the following to <tt class="FILENAME">C3.2.3.01.conf</tt>:

    # Example C3.2.3.01.conf
    # Example Pair rule.
    # Match event A and B within window.

    type=Pair
    ptype=RegExp
    pattern=foo
    desc=$0
    action=write - foo matched at %t. Start window of 5 seconds for bar ...
    ptype2=RegExp
    pattern2=bar
    desc2=$0
    action2=write - bar matched at %t.  bar is within window!
    window=5

Run with:

    <tt class="PROMPT">%</tt> <tt class="USERINPUT">**perl sec.pl -conf=C3.2.3.01.conf -input=-**</tt>

When running this rule, first enter ``<tt class="LITERAL">foo</tt>'' and ``<tt class="LITERAL">bar</tt>'' close together (i.e. within 5 seconds). Then enter ``<tt class="LITERAL">foo</tt>'' and wait to enter ``<tt class="LITERAL">bar</tt>'' until the
window is past (i.e. more than 5 seconds.) The first time the <span class="emphasis"><i class="EMPHASIS">Pair*</i></span> rule will correlate them together, while the second time
they are not correlated.

Output will look similar to:

    jpb@jpb-lt:~/SEC-examples$perl sec.pl -conf=C3.2.3.01.conf -input=-
    Simple Event Correlator version 2.1.11
    Reading configuration from C3.2.3.01.conf
    1 rules loaded from C3.2.3.01.conf
    <tt class="USERINPUT">**foo**</tt>
    Writing event 'foo matched at Sat Nov 15 18:17:07 2003. Start window of 5 seconds for bar ...' to file -
    foo matched at Sat Nov 15 18:17:07 2003. Start window of 5 seconds for bar ...
    <tt class="USERINPUT">**bar**</tt>
    Writing event 'bar matched at Sat Nov 15 18:17:09 2003.  bar is within window!' to file -
    bar matched at Sat Nov 15 18:17:09 2003.  bar is within window!
    <tt class="USERINPUT">**foo**</tt>
    Writing event 'foo matched at Sat Nov 15 18:17:14 2003. Start window of 5 seconds for bar ...' to file -
    foo matched at Sat Nov 15 18:17:14 2003. Start window of 5 seconds for bar ...
    <tt class="USERINPUT">**bar**</tt>
    ^C

</div>

<div class="SECT3">

* * *

### [3.2.4
PairWithWindow]()

At first glance, the <span class="emphasis"><i class="EMPHASIS">PairWithWindow*</i></span> rule appears identical to the <span class="emphasis"><i class="EMPHASIS">Pair*</i></span> rule. Both contain two patterns, two
actions, and a time window.

The difference is that, in <span class="emphasis"><i class="EMPHASIS">PairWithWindow*</i></span> the <span class="emphasis"><i class="EMPHASIS">action2*</i></span> is executed if events A and B both occur within the
time window. If A occurs, but B does not occur, then <span class="emphasis"><i class="EMPHASIS">action*</i></span> is executed.

Copy the following to <tt class="FILENAME">C3.2.4.01.conf</tt>:

    # Example C3.2.4.01.conf
    # Example PairWithWindow rule.
    # Match both events A and B within window executes action2.
    # If event B does not occur within window, execute action.

    type=PairWithWindow
    ptype=RegExp
    pattern=foo
    desc=$0
    action=write - foo matched, bar NOT matched within window.
    ptype2=RegExp
    pattern2=bar
    desc2=$0
    action2=write - foo and bar both matched within 5 second window!
    window=5

Run with:

    <tt class="PROMPT">%</tt> <tt class="USERINPUT">**perl sec.pl -conf=C3.2.4.01.conf -input=-**</tt>

When running this rule, first enter ``<tt class="LITERAL">foo</tt>'' and ``<tt class="LITERAL">bar</tt>'' close together (i.e. within 5 seconds). Then enter ``<tt class="LITERAL">foo</tt>'' and wait to enter ``<tt class="LITERAL">bar</tt>'' until the
window is past (i.e. more than 5 seconds.) The first time the <span class="emphasis"><i class="EMPHASIS">PairWithWindow*</i></span> rule will correlate them together, while the
second time they are not correlated.

Output looks like:

    jpb@jpb-lt:~/SEC-examples$perl sec.pl -conf=C3.2.4.01.conf -input=-
    Simple Event Correlator version 2.1.11
    Reading configuration from C3.2.4.01.conf
    1 rules loaded from C3.2.4.01.conf
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**bar**</tt>
    Writing event 'foo and bar both matched within 5 second window!' to file -
    foo and bar both matched within 5 second window!

    <tt class="USERINPUT">**foo**</tt>
    Writing event 'foo matched, bar NOT matched within window.' to file -
    foo matched, bar NOT matched within window.
    ^C

</div>

<div class="SECT3">

* * *

### [3.2.5
SingleWithThreshold]()

The <span class="emphasis"><i class="EMPHASIS">SingleWithThreshold*</i></span> rule is
used to ``count'' the number of matched events within a time window. If the number
exceeds the threshold, the <span class="emphasis"><i class="EMPHASIS">action*</i></span>
is executes

If the number of matched events, does not exceed the threshold within the time window,
the time window ``slides''- that is, start time for the correlation window is moved to
the second occurrence of the matched pattern. This process repeats, until the time window
expires with no new matched events.

Copy the following to <tt class="FILENAME">C3.2.5.01.conf</tt>:

    # Example C3.2.5.01.conf
    # Example SingleWithThreshold rule.
    # Match event A thresh number of times in window
    # and execute action.  Slide window if needed
    # until window expires.

    type=SingleWithThreshold
    ptype=RegExp
    pattern=foo
    desc=$0
    action=write - foo matched three times in 10 seconds!
    window=10
    thresh=3

Run with:

    <tt class="PROMPT">%</tt> <tt class="USERINPUT">**perl sec.pl -conf=C3.2.5.01.conf -input=-**</tt>

When running this rule, first enter ``<tt class="LITERAL">foo</tt>'' three times close
together (i.e. within 10 seconds). The <span class="emphasis"><i class="EMPHASIS">action*</i></span> will execute.

Then enter ``<tt class="LITERAL">foo</tt>'' slowly, waiting five to eight seconds
between each entry. Since there are never three entries (<span class="emphasis"><i class="EMPHASIS">thresh=3*</i></span>) entered within the sliding window, the rule is not
matched and the <span class="emphasis"><i class="EMPHASIS">action*</i></span> is not
executed.

The first time the <span class="emphasis"><i class="EMPHASIS">SingleWithThreshold*</i></span> rule will correlate them together, while
the second time they are not correlated.

Output looks similar to:

    jpb@jpb-lt:~/SEC-examples$perl sec.pl -conf=C3.2.5.01.conf -input=-
    Simple Event Correlator version 2.1.11
    Reading configuration from C3.2.5.01.conf
    1 rules loaded from C3.2.5.01.conf
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    Writing event 'foo matched three times in 10 seconds!' to file -
    foo matched three times in 10 seconds!

    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    ^C

</div>

<div class="SECT3">

* * *

### [3.2.6
SingleWith2Threshds]()

The <span class="emphasis"><i class="EMPHASIS">SingleWith2Thresholds*</i></span> rule
is very similar to the <span class="emphasis"><i class="EMPHASIS">SingleWithThreshold*</i></span> rule, except that we can now definitely
determine when events stop. This is done with a second threshold and a second timer
window.

<span class="emphasis"><i class="EMPHASIS">SingleWith2Thresholds*</i></span> counts the
number of matched events and executes <span class="emphasis"><i class="EMPHASIS">action*</i></span> when the number is above <span class="emphasis"><i class="EMPHASIS">thresh*</i></span> events.

Once this low threshold (watermark) is reached, SEC starts <span class="emphasis"><i class="EMPHASIS">window2*</i></span> and counts additional matched events. When the number
of events falls below <span class="emphasis"><i class="EMPHASIS">thresh2*</i></span>
events within <span class="emphasis"><i class="EMPHASIS">window2*</i></span>, SEC executes
<span class="emphasis"><i class="EMPHASIS">action2*</i></span>.

Note that both windows are sliding windows- that is, the begining time of the window
moves to the time of the next match if the time window of the first match expires.

Copy the following to <tt class="FILENAME">C3.2.6.01.conf</tt>:

    # Example C3.2.6.01.conf
    # Example SingleWith2Threshholds rule.
    # Match thresh A events (go above low watermark) and execute action.
    # Then switch to thresh2 and window2 to count more A events.
    # If less than thresh2 A events occur in window2 (stay under high
    # watermark), execute action2.

    type=SingleWith2Thresholds
    ptype=RegExp
    pattern=foo
    desc=$0
    action=write - foo hit low watermark (3) at time %t
    window=5
    thresh=3
    desc2=$0
    action2=write - foo stayed under high watermark (5) at time %t
    window2=10
    thresh2=5

Run with:

    <tt class="PROMPT">%</tt> <tt class="USERINPUT">**perl sec.pl -conf=C3.2.6.01.conf -input=-**</tt>

In this example, it will be necessary to time your entries fairly closely. The first
example shows three matched events followed by <span class="emphasis"><i class="EMPHASIS">action*</i></span>, then less than <span class="emphasis"><i class="EMPHASIS">thresh2*</i></span> (5 events) within <span class="emphasis"><i class="EMPHASIS">window2*</i></span> (10 seconds).

The second example shows multiple events after the low watermark then a marked slowing
of entries, that results in <span class="emphasis"><i class="EMPHASIS">action2*</i></span>.

Output from this example:

    jpb@jpb-lt:~/SEC-examples$perl sec.pl -conf=C3.2.6.01.conf -input=-
    Simple Event Correlator version 2.1.11
    Reading configuration from C3.2.6.01.conf
    1 rules loaded from C3.2.6.01.conf
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    Writing event 'foo hit low watermark (3) at time Mon Nov 17 09:42:05 2003' to file -
    foo hit low watermark (3) at time Mon Nov 17 09:42:05 2003
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    Writing event 'foo stayed under high watermark (5) at time Mon Nov 17 09:42:16 2003' to file -
    foo stayed under high watermark (5) at time Mon Nov 17 09:42:16 2003

    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    Writing event 'foo hit low watermark (3) at time Mon Nov 17 09:42:22 2003' to file -
    foo hit low watermark (3) at time Mon Nov 17 09:42:22 2003
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo slowing way down...**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    Writing event 'foo stayed under high watermark (5) at time Mon Nov 17 09:42:49 2003' to file -
    foo stayed under high watermark (5) at time Mon Nov 17 09:42:49 2003
    ^C

</div>

<div class="SECT3">

* * *

### [3.2.7 Suppress]()

The <span class="emphasis"><i class="EMPHASIS">Suppress*</i></span> rule is very
intuitive- events matching the pattern are suppressed. Since this rule has no <span class="emphasis"><i class="EMPHASIS">action*</i></span> statement, it does nothing.

<span class="emphasis"><i class="EMPHASIS">Suppress*</i></span> is particularly useful
with the <span class="emphasis"><i class="EMPHASIS">context*</i></span> parameter.
Contexts are discussed in Section 4, and an additional example of <span class="emphasis"><i class="EMPHASIS">Suppress*</i></span> can be found there. This example
does not depend on the use of contexts.

Copy the following to <tt class="FILENAME">C3.2.07.01.conf</tt>:

    # Example C3.2.7.01.conf
    # Example of Suppress.
    # First rule suppresses 'foo'.
    # Second rule matches any pattern and
    # executes write action.

    type=Suppress
    ptype=RegExp
    pattern=foo
    desc=$0

    type=Single
    ptype=RegExp
    pattern=(.*)
    desc=$0
    action=write - entry was: $0

Run with:

    <tt class="PROMPT">%</tt> <tt class="USERINPUT">**perl sec.pl -conf=C3.2.7.01.conf -input=-**</tt>

In this example, the first rule suppresses ``<tt class="LITERAL">foo</tt>'' while the
second rule matches any pattern and writes it to standard output. Since ``<tt class="LITERAL">foo</tt>'' is already suppressed by the first rule, it will never be
written by the second rule.

Output looks similar to:

    jpb@jpb-lt:~/SEC-examples$perl sec.pl -conf=C3.2.7.01.conf -input=-    
    Simple Event Correlator version 2.1.11
    Reading configuration from C3.2.7.01.conf
    2 rules loaded from C3.2.7.01.conf
    <tt class="USERINPUT">**bar**</tt>
    Writing event 'entry was: bar' to file -
    entry was: bar
    <tt class="USERINPUT">**baz**</tt>
    Writing event 'entry was: baz' to file -
    entry was: baz
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**bar**</tt>
    Writing event 'entry was: bar' to file -
    entry was: bar
    <tt class="USERINPUT">**baz**</tt>
    Writing event 'entry was: baz' to file -
    entry was: baz
    ^C

</div>

<div class="SECT3">

* * *

### [3.2.8 Calendar]()

The <span class="emphasis"><i class="EMPHASIS">Calendar*</i></span> rule is another
easy to understand rule. It executes <span class="emphasis"><i class="EMPHASIS">action*</i></span> statements at specific times. The time specification
is similar to that used by cron(8), and is detailed in crontab(5).

Note that the time specifications do not include the Vixie cron extensions.

Copy the following to <tt class="FILENAME">C3.2.8.01.conf</tt>:

    # Example C3.2.8.01.conf
    # Example calendar rule.
    # Write a message every minute.

    type=Calendar
    time=* * * * *
    desc=$0
    action=write - The time is now: %t

This example takes no user input. However, the ``<tt class="LITERAL">-input</tt>''
parameter must still be present on the command line. Run with:

    <tt class="PROMPT">%</tt> <tt class="USERINPUT">**perl sec.pl -conf=C3.2.8.01.conf -input=-**</tt>

Output is similar to:

    jpb@jpb-lt:~/SEC-examples$perl sec.pl -conf=C3.2.8.01.conf -input=-
    Simple Event Correlator version 2.1.11
    Reading configuration from C3.2.8.01.conf
    1 rules loaded from C3.2.8.01.conf
    Writing event 'The time is now: Mon Nov 17 10:40:42 2003' to file -
    The time is now: Mon Nov 17 10:40:42 2003
    Writing event 'The time is now: Mon Nov 17 10:41:00 2003' to file -
    The time is now: Mon Nov 17 10:41:00 2003
    Writing event 'The time is now: Mon Nov 17 10:42:00 2003' to file -
    The time is now: Mon Nov 17 10:42:00 2003
    ^C

Using the ``<tt class="LITERAL">-debug=4</tt>'' parameter removes the informational
debug statements and results in just:

    The time is now: Mon Nov 17 10:46:35 2003
    The time is now: Mon Nov 17 10:47:00 2003
    The time is now: Mon Nov 17 10:48:00 2003
    The time is now: Mon Nov 17 10:49:00 2003
    The time is now: Mon Nov 17 10:50:00 2003

Note also that SEC invokes the <span class="emphasis"><i class="EMPHASIS">action*</i></span> of all calendar rules at startup, but only at the top
of each minute thereafter. Actions that must not occur too closely together must take
this into account.

Running applications from SEC is similar. This example runs a script that checks MD5
checksums on a list of files every five minutes. The script takes a single parameter-
``<tt class="LITERAL">MD5_CHECK</tt>'':

    #
    # Run the SystemCheck.sh script every five minutes.
    #
    type=Calendar
    time=0,5,10,15,20,25,30,35,40,45,50,55 * * * *
    desc=MD5_CHECK
    action=shellcmd /home/jpb/SEC-examples/SystemCheck.sh %s

</div>
</div>

<div class="SECT2">

* * *

## [3.3 Multiple
Input Streams]()

This section presents another use of the <span class="emphasis"><i class="EMPHASIS">spawn*</i></span> action- obtaining input from multiple input streams. As
shown in Section 2.2.3, <span class="emphasis"><i class="EMPHASIS">spawn*</i></span>
starts another program and redirects the program output as input to SEC. Both the
original input stream and this new input stream are processed by SEC as a single input
stream.

To set up SEC to read multiple files, the <b class="APPLICATION">tail** program is
often used as in the following example.

Copy the following to <tt class="FILENAME">C3.3.01.conf</tt>:

    #
    # Example C3.3.01.conf
    # Multiple input files with spawn.
    #
    #
    type=Single
    ptype=RegExp
    pattern=foo
    continue=TakeNext
    desc=$0
    action=spawn /usr/bin/tail -f ./aaa.in ;\
           spawn /usr/bin/tail -f ./bbb.in ;\
           spawn /usr/bin/tail -f ./ccc.in ;

    # Match lines beginning with aaa:
    type=Single
    ptype=RegExp
    pattern=^aaa:(.*)
    desc=$0
    action=write aaa.out %s

    # Match lines beginning with bbb:
    type=Single
    ptype=RegExp
    pattern=^bbb:(.*)
    desc=$0
    action=write bbb.out %s

    # Match lines beginning with ccc:
    type=Single
    ptype=RegExp
    pattern=^ccc:(.*)
    desc=$0
    action=write ccc.out %s

    # Match all other lines
    type=Single
    ptype=RegExp
    pattern=(.*)
    desc=$0
    action=write other.out %s

In this example, the <span class="emphasis"><i class="EMPHASIS">spawn*</i></span>
action is part of a rule that matches input ``<tt class="LITERAL">foo</tt>''. This means
that the spawn actions will not occur until ``<tt class="LITERAL">foo</tt>'' is
recognized in the input stream.

After the <span class="emphasis"><i class="EMPHASIS">tail*</i></span> commands will
forward input from their respective files into SEC. SEC will treat all input streams the
same, and parse input from all streams according to all rules.

Note that the input files <tt class="FILENAME">aaa.in</tt>, <tt class="FILENAME">bbb.in</tt>, and <tt class="FILENAME">ccc.in</tt> must exist before
running the example. Use the <b class="APPLICATION">touch** command to create these
empty files as follows:

    <tt class="PROMPT">%</tt> <tt class="USERINPUT">**touch aaa.in bbb.in ccc.in**</tt>

Note also that the last rule is a catch-all rule: if the input does not get recognized
by any other rule, it will be written to <tt class="FILENAME">other.out</tt>

Run with:

    <tt class="PROMPT">%</tt> <tt class="USERINPUT">**perl sec.pl -conf=C3.3.01.conf -input=-**</tt>

The session starts as follows:

    jpb@jpb-lt:~/SEC-examples/tmp$perl ../sec.pl -conf=C3.3.01.conf -input=-
    Simple Event Correlator version 2.1.11
    Reading configuration from C3.3.01.conf
    5 rules loaded from C3.3.01.conf
    <tt class="USERINPUT">**foo**</tt>
    Spawning shell command '/usr/bin/tail -f ./aaa.in'
    Child 15940 created for command '/usr/bin/tail -f ./aaa.in'
    Spawning shell command '/usr/bin/tail -f ./bbb.in'
    Child 15941 created for command '/usr/bin/tail -f ./bbb.in'
    Spawning shell command '/usr/bin/tail -f ./ccc.in'
    Child 15942 created for command '/usr/bin/tail -f ./ccc.in'
    Writing event 'foo' to file other.out
    <tt class="USERINPUT">**aaa:input from terminal**</tt>
    Writing event 'aaa:input from terminal' to file aaa.out
    <tt class="USERINPUT">**bbb:input from terminal**</tt>
    Writing event 'bbb:input from terminal' to file bbb.out
    <tt class="USERINPUT">**ccc:input from terminal**</tt>
    Writing event 'ccc:input from terminal' to file ccc.out
    <tt class="USERINPUT">**ddd:input from terminal**</tt>
    Writing event 'ddd:input from terminal' to file other.out

So far all input has been from the terminal. In another window or session in the same
directory, perform the following commands:

    <tt class="PROMPT">%</tt> <tt class="USERINPUT">**echo "aaa:from other session copied into ccc.in" >> ccc.in**</tt>

    <tt class="PROMPT">%</tt> <tt class="USERINPUT">**echo "bbb:from other session copied into aaa.in" >> aaa.in**</tt>

    <tt class="PROMPT">%</tt> <tt class="USERINPUT">**echo "ddd:from other session copied into bbb.in" >> bbb.in**</tt>

SEC processes these inputs as well:

    Creating event 'aaa:from other session copied into ccc.in' (received from child 15956)
    Writing event 'aaa:from other session copied into ccc.in' to file aaa.out
    Creating event 'bbb:from other session copied into aaa.in' (received from child 15954)
    Writing event 'bbb:from other session copied into aaa.in' to file bbb.out
    Creating event 'ddd:from other session copied into bbb.in' (received from child 15955)
    Writing event 'ddd:from other session copied into bbb.in' to file other.out
    ^C

Examine each output file to determine its contents:

    jpb@jpb-lt:~/SEC-examples/tmp$cat aaa.out
    aaa:input from terminal
    aaa:from other session copied into ccc.in

    jpb@jpb-lt:~/SEC-examples/tmp$cat bbb.out
    bbb:input from terminal
    bbb:from other session copied into aaa.in

    jpb@jpb-lt:~/SEC-examples/tmp$cat ccc.out
    ccc:input from terminal

    jpb@jpb-lt:~/SEC-examples/tmp$cat other.out
    foo
    ddd:input from terminal
    ddd:from other session copied into bbb.in

As shown above, SEC parsed the input, regardless of where it came from, and performed
the actions indicated on each matched rule.

</b></b></div>

<div class="SECT2">

* * *

## [3.4 Pipe Output]()

This section looks at another feature of the <span class="emphasis"><i class="EMPHASIS">write*</i></span> action; writing to a ``named pipe'', also called a
``fifo''. This feature provides a simple method of inter-process communication (IPC).

Most Unix systems already have the ability to create and use named pipes. With SEC,
the only requirement is that the named pipe must exist before writing to it. Typically
this is performed with the <b class="APPLICATION">mkfifo** or <b class="APPLICATION">mknod** command. Check your system documentation for how to create
a named pipe. On BSD based systems, the command is:

    <tt class="PROMPT">%</tt> <tt class="USERINPUT">**mkfifo SEC_fifo**</tt>

This creates a named pipe in the current directory:

    prw-r--r--  1 jpb  staff  -    0 Nov 15 12:38 SEC_fifo

To be useful, there must be a way to get data out of the named pipe. Copy the
following perl script to file <tt class="FILENAME">S3.4.01.pl</tt>:

    #!/usr/bin/perl
    #
    # Example S3.4.01.pl - Script to read data out of a named pipe.
    #
    $| = 1;
    $filename = "./SEC_fifo";
    open(FIFO, "+< $filename") or die "fifo  $!";
    print "Start of S3.4.01.pl.  Reading $filename...\n";

    while (<FIFO>)
    {
      $inputline = $_;
      print "Got: $inputline";
    }

This script simply copies whatever it receives from the named pipe to standard
output.

Run with:

    <tt class="PROMPT">%</tt> <tt class="USERINPUT">**perl S3.4.01.pl**</tt>

The program announces its startup banner and waits for input:

    jpb@jpb-lt:~/SEC-examples/tmp$perl S3.4.01.pl
    Start of S3.4.01.pl.  Reading ./SEC_fifo...

In a separate window or session, create <tt class="FILENAME">C3.4.01.conf</tt> with
the following rule:

    # Example C3.4.01.conf
    # Recognize any pattern and write to a FIFO.
    #
    type=Single
    ptype=RegExp
    pattern=(.*)
    desc=$0
    action=write SEC_fifo %s

Run with:

    <tt class="PROMPT">%</tt> <tt class="USERINPUT">**perl sec.pl -conf=C3.4.01.conf -input=-**</tt>

and type some text:

    jpb@jpb-lt:~/SEC-examples$perl sec.pl -conf=C3.4.01.conf -input=-
    Simple Event Correlator version 2.1.11
    Reading configuration from C3.4.01.conf
    1 rules loaded from C3.4.01.conf
    <tt class="USERINPUT">**This is**</tt>
    Writing event 'This is' to file SEC_fifo
    <tt class="USERINPUT">**a test of**</tt>
    Writing event 'a test of' to file SEC_fifo
    <tt class="USERINPUT">**SEC named pipes.**</tt>
    Writing event 'SEC named pipes.' to file SEC_fifo
    ^C

The output from the reader script looks like:

    jpb@jpb-lt:~/SEC-examples$perl S3.4.01.pl
    Start of S3.4.01.pl.  Reading ./SEC_fifo...
    Got: This is
    Got: a test of
    Got: SEC named pipes.
    ^C

SEC will complain if it can't write to the pipe. In this example, the reading script
was manually terminated after the first input line. All remaining lines received an
error:

    jpb@jpb-lt:~/SEC-examples$perl sec.pl -conf=C3.4.01.conf -input=-
    Simple Event Correlator version 2.1.11
    Reading configuration from C3.4.01.conf
    1 rules loaded from C3.4.01.conf
    <tt class="USERINPUT">**This is**</tt>
    Writing event 'This is' to file SEC_fifo
    <tt class="USERINPUT">**another test**</tt>
    Writing event 'another test' to file SEC_fifo
    Can't open pipe SEC_fifo for writing event 'another test'!
    <tt class="USERINPUT">**of SEC named pipes.**</tt>
    Writing event 'of SEC named pipes.' to file SEC_fifo
    Can't open pipe SEC_fifo for writing event 'of SEC named pipes.'!
    ^C

While trivial, this example illustrates the basic mechanism for passing event
correlation analysis to another program. Part Two of this article contains an example of
joining SEC with a database system and uses this technique to pass input into the
database.

</b></b></div>
</b></div>

<div class="SECT1">

* * *

# [4 Using Contexts]()

SEC has the ability to define and use <span class="emphasis"><i class="EMPHASIS">contexts*</i></span> with rules. A context is ``the interrelated
conditions in which something exists or occurs''. In SEC, a context ``exists or occurs''
when it is created by a rule action. In addition to the actions covered in Section 2.2,
SEC provides additional actions that create, destroy, add to, report on, or otherwise
manipulate a context.

Contexts can act as event stores. Events can be added to contexts as they occur. A
collection of events in a context can be input to a script to be saved in a file.

Initially, contexts might be hard to mentally grasp. They are a logical instance of
data. They act more like perl variables- springing into existence when created. In fact,
they are implemented as perl hashes. Their chief characteristic is their logical
existence as they relate to rules.

Most rules have a ``<tt class="LITERAL">context</tt>'' parameter that is used to
interact with contexts. SEC actually uses this parameter in two different ways. The
operand is either:

*   

A logical expression containing a context name

*   

A perl mini-program if the operand begins with a ``=''

Both types are shown below.

    
Below is a  rule that uses a context parameter:
    #
    type=Single
    ptype=RegExp
    pattern=foo
    context=FOO_CONTEXT  [![(1)](./imagelib/callouts/1.png)]()
    desc=$0
    action=logonly

    Below is a rule that uses a context mini-program:

    #
    type=Single
    ptype=RegExp
    pattern=foo
    context= =({my $var = 1; return $var})  [![(2)](./imagelib/callouts/2.png)]()
    desc=$0
    action=logonly

<div class="CALLOUTLIST">
<dl compact="COMPACT">
<dt>[![(1)](./imagelib/callouts/1.png)](#CONTEXTNAME)</dt>

<dd><tt class="LITERAL">FOO_CONTEXT</tt> is a context name. The name is tested to see if
the context exists as a part of evaluating whether the rule matches the input.</dd>

<dd>

This rule says: 

> Match the pattern ``foo'' and if the context named ``FOO_CONTEXT'' exists, execute the
> action statement.

</dd>

<dd>

</dd>

<dt>[![(2)](./imagelib/callouts/2.png)](#CONTEXTMINIPROGRAM)</dt>

<dd>This <span class="emphasis"><i class="EMPHASIS">context*</i></span> statement contains
a perl mini-program. The operand begins with a ``=''. The perl mini-program is evaluated
(with the perl <b class="APPLICATION">eval** statement) and the return value is
checked. If true (non zero), the <span class="emphasis"><i class="EMPHASIS">context*</i></span> parameter is considered true.</b></dd>
</dl>
</div>

A natural question is: ``How do contexts get created in the first place?'' The answer
is that they are created by another rule.

<div class="SECT2">

* * *

## [4.1 Simple
Context Example]()

The first example in this section simply checks for the existence of a context. If the
context exists, the rule is executed. In order to create the context, a second rule is
needed, with the <span class="emphasis"><i class="EMPHASIS">create*</i></span> action to
create the context for the first rule.

Copy the following to <tt class="FILENAME">C4.1.01.conf</tt>:

    # Example C4.1.01.conf
    # Context example.
    # Write action if context named FOO_CONTEXT exists.
    # Create context with second rule, matching 'bar'
    #

    type=Single
    ptype=RegExp
    pattern=foo
    context=FOO_CONTEXT
    desc=$0
    action=write - context exists, writing foo here

    type=Single
    ptype=RegExp
    pattern=bar
    desc=$0
    action=create FOO_CONTEXT

In this example, ``foo'' is recognized by the pattern in the first rule, but the rule
does not trigger until the context <tt class="LITERAL">FOO_CONTEXT</tt> is created. The
context is created by the second rule which matches the ``bar'' pattern, and has an
action to create context <tt class="LITERAL">FOO_CONTEXT</tt>.

Run with:

    <tt class="PROMPT">%</tt> <tt class="USERINPUT">**perl sec.pl -conf=C4.1.01.conf -input=-**</tt>

Output looks like:

    jpb@jpb-lt:~/SEC-examples$perl sec.pl -conf=C4.1.01.conf -input=-
    Simple Event Correlator version 2.1.11
    Reading configuration from C4.1.01.conf
    2 rules loaded from C4.1.01.conf
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**bar**</tt>
    Creating context 'FOO_CONTEXT'
    <tt class="USERINPUT">**foo**</tt>
    Writing event 'context exists, writing foo here' to file -
    context exists, writing foo here
    <tt class="USERINPUT">**foo**</tt>
    Writing event 'context exists, writing foo here' to file -
    context exists, writing foo here
    <tt class="USERINPUT">**foo**</tt>
    Writing event 'context exists, writing foo here' to file -
    context exists, writing foo here
    ^C

Once the second rule was matched, and the <span class="emphasis"><i class="EMPHASIS">create*</i></span> action was executed, the context <tt class="LITERAL">FOO_CONTEXT</tt> sprang into existence. After that, the first rule was
triggered when pattern ``<tt class="LITERAL">foo</tt>'' was matched in the input
stream.

<div class="SECT3">

* * *

### [4.1.1 Context
Lifetimes]()

In the example above, the <span class="emphasis"><i class="EMPHASIS">create*</i></span>
action created context <tt class="LITERAL">FOO_CONTEXT</tt> with, by default, infinite
lifetime. The <span class="emphasis"><i class="EMPHASIS">create*</i></span> action
actually has the following syntax:

<tt class="LITERAL">create [<name> [<time> [<action list>] ]
]</tt>

The <span class="emphasis"><i class="EMPHASIS">name*</i></span> is the name of the
context. <span class="emphasis"><i class="EMPHASIS">time*</i></span> is the lifetime of
the context in seconds. The <span class="emphasis"><i class="EMPHASIS">action
list*</i></span> parameter is a list of any SEC action statements- each action statement
separated by semi-colons, ``;''.

To see the effect of the <span class="emphasis"><i class="EMPHASIS">time*</i></span>
(context lifetime) parameter, change the line in <tt class="FILENAME">C4.1.01.conf</tt>
from

    action=create FOO_CONTEXT

to

    action=create FOO_CONTEXT 5

This creates context <tt class="LITERAL">FOO_CONTEXT</tt> with a lifetime of 5
seconds. At the end of its lifetime, the context expires, at which point it is no longer
valid. SEC notes the context expiration in its informative debug output.

Re-running the above example:

    jpb@jpb-lt:~/SEC-examples$perl sec.pl -conf=C4.1.01.conf -input=-
    Simple Event Correlator version 2.1.11
    Reading configuration from C4.1.01.conf
    2 rules loaded from C4.1.01.conf
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**bar**</tt>
    Creating context 'FOO_CONTEXT'
    <tt class="USERINPUT">**foo**</tt>
    Writing event 'context exists, writing foo here' to file -
    context exists, writing foo here
    <tt class="USERINPUT">**foo**</tt>
    Writing event 'context exists, writing foo here' to file -
    context exists, writing foo here
    Deleting stale context 'FOO_CONTEXT'
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    ^C

Here context <tt class="LITERAL">FOO_CONTEXT</tt> did not exist until the second rule
was matched (by matching ``bar'') at which time it was created with a lifetime of five
seconds.

During its five second lifetime the first rule pattern ``foo'' was successfully
matched twice and the action statement for the first rule executed twice.

At the end of its lifetime, context <tt class="LITERAL">FOO_CONTEXT</tt> expired,
which is noted by the SEC informative debug output ``<tt class="LITERAL">Deleting stale
context 'FOO_CONTEXT'</tt>''.

After the context expired, the first rule is no longer successfully triggered, even
though ``foo'' is matched, because context <tt class="LITERAL">FOO_CONTEXT</tt> no longer
exists.

Contexts are one of the most powerful features of SEC. It is possible to develop
complex and intricate event correlation scenarios by combining pattern matching, context
creation and manipulation, and actions.

</div>

<div class="SECT3">

* * *

### [4.1.2 <span class="emphasis"><i class="EMPHASIS">create*</i></span> Action Lists]()

As shown above, contexts themselves can have an <span class="emphasis"><i class="EMPHASIS">action list*</i></span> that is part of the <span class="emphasis"><i class="EMPHASIS">create*</i></span> statement. The <span class="emphasis"><i class="EMPHASIS">action list*</i></span> is a list of any SEC permissible actions.

Action lists on the <span class="emphasis"><i class="EMPHASIS">create*</i></span>
statement are executed when the context expires. This allows for contexts to perform
actions just before they disappear. The next example shows this usage.

Copy the following to <tt class="FILENAME">C4.1.02.conf</tt>:

    # Example C4.1.02.conf
    # Context example.
    # Write action if context named FOO_CONTEXT exists.
    # Create context with second rule, matching 'bar'
    #

    type=Single
    ptype=RegExp
    pattern=foo
    context=FOO_CONTEXT
    desc=$0
    action=write - context exists, writing foo here

    type=Single
    ptype=RegExp
    pattern=bar
    desc=$0
    action=create FOO_CONTEXT 10 (write - Context FOO_CONTEXT is ending; write - Goodbye...;)

This configuration is almost identical to <tt class="FILENAME">C4.1.01.conf</tt> with
the exception of an <span class="emphasis"><i class="EMPHASIS">action list*</i></span> on
the <span class="emphasis"><i class="EMPHASIS">create*</i></span> statement. When context
<tt class="LITERAL">FOO_CONTEXT</tt> expires after 10 seconds, it performs two <span class="emphasis"><i class="EMPHASIS">write*</i></span> actions.

Run with:

    <tt class="PROMPT">%</tt> <tt class="USERINPUT">**perl sec.pl -conf=C4.1.02.conf -input=-**</tt>

Output looks like:

    jpb@jpb-lt:~/SEC-examples$perl sec.pl -conf=C4.1.02.conf -input=-
    Simple Event Correlator version 2.1.11
    Reading configuration from C4.1.02.conf
    2 rules loaded from C4.1.02.conf
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**bar**</tt>
    Creating context 'FOO_CONTEXT'
    <tt class="USERINPUT">**foo**</tt>
    Writing event 'context exists, writing foo here' to file -
    context exists, writing foo here
    <tt class="USERINPUT">**foo**</tt>
    Writing event 'context exists, writing foo here' to file -
    context exists, writing foo here
    <tt class="USERINPUT">**foo**</tt>
    Writing event 'context exists, writing foo here' to file -
    context exists, writing foo here
    Deleting stale context 'FOO_CONTEXT'
    Writing event 'Context FOO_CONTEXT is ending' to file -
    Context FOO_CONTEXT is ending
    Writing event 'Goodbye...' to file -
    Goodbye...
    ^C

Just before context <tt class="LITERAL">FOO_CONTEXT</tt> ended, its <span class="emphasis"><i class="EMPHASIS">action list*</i></span> was executed, resulting in
the two <span class="emphasis"><i class="EMPHASIS">write*</i></span> statement sending
their text to standard output. Without the informative debug output, the results look
like:

    jpb@jpb-lt:~/SEC-examples$perl sec.pl -conf=C4.1.02.conf -input=- -debug=4
    Simple Event Correlator version 2.1.11
    Reading configuration from C4.1.02.conf
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**bar**</tt>
    <tt class="USERINPUT">**foo**</tt>
    context exists, writing foo here
    <tt class="USERINPUT">**foo**</tt>
    context exists, writing foo here
    Context FOO_CONTEXT is ending
    Goodbye...
    ^C

</div>
</div>

<div class="SECT2">

* * *

## [4.2 Other Actions Pertaining To Contexts]()

So far, contexts have been used as simple boolean operators- they exist or they do not
exist. Contexts have an additional, very useful feature- they can store events. This
makes them ideal for event reporting.

Context parameters, such as the lifetime and the action list can also be changed. The
context name however, can not be changed.

This section presents additional actions that are useful in manipulating contexts.

<div class="SECT3">

* * *

### [4.2.1 Set]()

<tt class="LITERAL">set <name> <time> [<action list>]</tt>

<span class="emphasis"><i class="EMPHASIS">set*</i></span> is used to change the
settings for a context. The context identified by <span class="emphasis"><i class="EMPHASIS">name*</i></span> must already exist. Use <span class="emphasis"><i class="EMPHASIS">set*</i></span> to change the <span class="emphasis"><i class="EMPHASIS">time*</i></span> and/or the <span class="emphasis"><i class="EMPHASIS">action list*</i></span> for the context.

Typically, a simple context will be created with <span class="emphasis"><i class="EMPHASIS">create*</i></span> which will be later <span class="emphasis"><i class="EMPHASIS">set*</i></span> with the desired lifetime and action list. Below is an
example:

Copy the following to <tt class="FILENAME">C4.2.01.conf</tt>:

    # Example C4.2.01.conf
    # Context example.
    # Create context with second rule, matching 'bar'
    # Use set action to extend life of context with first rule.
    #

    type=Single
    ptype=RegExp
    pattern=foo
    context=FOO_CONTEXT
    desc=$0
    action=write - setting context FOO_CONTEXT lifetime to 10 seconds at %t; \
           set FOO_CONTEXT 10 (write - Context FOO_CONTEXT died at %t)

    type=Single
    ptype=RegExp
    pattern=bar
    desc=$0
    action=create FOO_CONTEXT; write - creating context FOO_CONTEXT;

Run with the <tt class="LITERAL">-debug=4</tt> parameter to skip the informative
output:

    <tt class="PROMPT">%</tt> <tt class="USERINPUT">**perl sec.pl -conf=C4.2.01.conf -input=- -debug=4**</tt>

Output looks like:

    jpb@jpb-lt:~/SEC-examples$perl sec.pl -conf=C4.2.01.conf -input=- -debug=4
    Simple Event Correlator version 2.1.11
    Reading configuration from C4.2.01.conf
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**foo**</tt>
    <tt class="USERINPUT">**bar**</tt>
    creating context FOO_CONTEXT
    <tt class="USERINPUT">**foo**</tt>
    setting context FOO_CONTEXT lifetime to 10 seconds at Mon Nov 17 22:37:17 2003
    <tt class="USERINPUT">**foo**</tt>
    setting context FOO_CONTEXT lifetime to 10 seconds at Mon Nov 17 22:37:21 2003
    <tt class="USERINPUT">**foo**</tt>
    setting context FOO_CONTEXT lifetime to 10 seconds at Mon Nov 17 22:37:24 2003
    <tt class="USERINPUT">**foo**</tt>
    setting context FOO_CONTEXT lifetime to 10 seconds at Mon Nov 17 22:37:29 2003
    <tt class="USERINPUT">**foo**</tt>
    setting context FOO_CONTEXT lifetime to 10 seconds at Mon Nov 17 22:37:35 2003
    Context FOO_CONTEXT died at Mon Nov 17 22:37:46 2003
    ^C

This example creates a context (with the second rule) then keeps extending the
lifetime of the context (with the first rule) to 10 seconds every time the first rule is
triggered. Only when there is no more ``<tt class="LITERAL">foo</tt>'' input does the
context finally expire.

</div>

<div class="SECT3">

* * *

### [4.2.2 Add and Report]()

<tt class="LITERAL">add <name> [<event text>]</tt>

<tt class="LITERAL">report <name> [<shellcmd>]</tt>

The <span class="emphasis"><i class="EMPHASIS">add*</i></span> action adds an event to
a context. The context parameters do not change, only the contents of the context. Think
of the context as a box- <span class="emphasis"><i class="EMPHASIS">add*</i></span> adds
events as they occur to the box. Other actions in this section (<span class="emphasis"><i class="EMPHASIS">delete*</i></span>, <span class="emphasis"><i class="EMPHASIS">fill*</i></span>,<span class="emphasis"><i class="EMPHASIS">report*</i></span>, <span class="emphasis"><i class="EMPHASIS">copy*</i></span>,and <span class="emphasis"><i class="EMPHASIS">empty*</i></span>) operate on the contents of that box.

While its nice to have the capability to add events to a context, its necessary to
have a way to get them out. The <span class="emphasis"><i class="EMPHASIS">report*</i></span> action can be used to list the contents of a context
through a system command (<span class="emphasis"><i class="EMPHASIS">shellcmd*</i></span>). SEC runs the named <span class="emphasis"><i class="EMPHASIS">shellcmd*</i></span> (subject to normal user permissions) and passes the
content of the context to standard input of the command.

The following example creates a context (<tt class="LITERAL">MY_CONTEXT</tt>), adds
events to it, and reports the contents with <b class="APPLICATION">/bin/cat**.

The first attempt is shown in <tt class="FILENAME">C4.2.02.conf</tt>:

    # Example C4.2.02.conf
    # Rule 1: create context MY_CONTEXT
    # Rule 2: Add to context MY_CONTEXT
    # Rule 3: report context MY_CONTEXT
    # Third rule reports context
    #

    # Pattern 'CreateMe' creates the action
    type=Single
    ptype=RegExp
    pattern=CreateMe
    continue=TakeNext
    desc=$0
    action=create MY_CONTEXT

    # Anything else gets added to context 'MY_CONTEXT'
    type=Single
    ptype=RegExp
    pattern=(.*)
    desc=$0
    action=add MY_CONTEXT $0

    # Pattern 'ReportMe' executes the report action
    type=Single
    ptype=RegExp
    pattern=ReportMe
    context=MY_CONTEXT
    desc=$0
    action=report MY_CONTEXT /bin/cat

When run, this attempt looks like:

    jpb@jpb-lt:~/SEC-examples$perl sec.pl -conf=C4.2.02.conf -input=-
    Simple Event Correlator version 2.1.11
    Reading configuration from C4.2.02.conf
    3 rules loaded from C4.2.02.conf
    <tt class="USERINPUT">**CreateMe**</tt>
    Creating context 'MY_CONTEXT'
    Adding event 'CreateMe' to context 'MY_CONTEXT'
    <tt class="USERINPUT">**line one**</tt>
    Adding event 'line one' to context 'MY_CONTEXT'
    <tt class="USERINPUT">**line two**</tt>
    Adding event 'line two' to context 'MY_CONTEXT'
    <tt class="USERINPUT">**ReportMe**</tt>
    Adding event 'ReportMe' to context 'MY_CONTEXT'
    <tt class="USERINPUT">**ReportMe**</tt>
    Adding event 'ReportMe' to context 'MY_CONTEXT'
    <tt class="USERINPUT">**???**</tt>
    Adding event '???' to context 'MY_CONTEXT'
    ^C

What is happening in this case is that the pattern match for rule 2 ``<tt class="LITERAL">(.*)</tt>'' is swallowing the input ``<tt class="LITERAL">ReportMe</tt>''
before it gets to the third rule.

A possible fix is to add <tt class="LITERAL">continue=TakeNext</tt> to rule 2:

    # Anything else gets added to context 'MY_CONTEXT'
    type=Single
    ptype=RegExp
    pattern=(.*)
    continue=TakeNext
    desc=$0
    action=add MY_CONTEXT $0

This produces:

    jpb@jpb-lt:~/SEC-examples$perl sec.pl -conf=C4.2.02.conf -input=-
    Simple Event Correlator version 2.1.11
    Reading configuration from C4.2.02.conf
    3 rules loaded from C4.2.02.conf
    <tt class="USERINPUT">**CreateMe**</tt>
    Creating context 'MY_CONTEXT'
    Adding event 'CreateMe' to context 'MY_CONTEXT'
    <tt class="USERINPUT">**line one**</tt>
    Adding event 'line one' to context 'MY_CONTEXT'
    <tt class="USERINPUT">**line two**</tt>
    Adding event 'line two' to context 'MY_CONTEXT'
    <tt class="USERINPUT">**ReportMe**</tt>
    Adding event 'ReportMe' to context 'MY_CONTEXT'
    Reporting the event store of context 'MY_CONTEXT' through shell command '/bin/cat'
    Child 2180 created for command '/bin/cat'
    CreateMe
    line one
    line two
    ReportMe
    ^C

This works, but the ``<tt class="LITERAL">CreateMe</tt>'' and ``<tt class="LITERAL">ReportMe</tt>'' input ends up in the context and itself is reported.

A bit more elegant solution is to reverse the order of rules 2 and 3. And now, no <tt class="LITERAL">continue=TakeNext</tt> is needed:

    # Example C4.2.02.conf
    # Rule 1: create context MY_CONTEXT
    # Rule 2: report context MY_CONTEXT
    # Rule 3: Add to context MY_CONTEXT
    # Third rule reports context
    #

    # Pattern 'CreateMe' creates the action
    type=Single
    ptype=RegExp
    pattern=CreateMe
    desc=$0
    action=create MY_CONTEXT

    # Pattern 'ReportMe' executes the report action
    type=Single
    ptype=RegExp
    pattern=ReportMe
    context=MY_CONTEXT
    desc=$0
    action=report MY_CONTEXT /bin/cat

    # Anything else gets added to context 'MY_CONTEXT'
    type=Single
    ptype=RegExp
    pattern=(.*)
    desc=$0
    action=add MY_CONTEXT $0

Without the informative debug statements the output is:

    jpb@jpb-lt:~/SEC-examples$perl sec.pl -conf=C4.2.02.conf -input=- -debug=4
    Simple Event Correlator version 2.1.11
    Reading configuration from C4.2.02.conf
    <tt class="USERINPUT">**CreateMe**</tt>
    <tt class="USERINPUT">**line one**</tt>
    <tt class="USERINPUT">**line two**</tt>
    <tt class="USERINPUT">**ReportMe**</tt>
    line one
    line two
    ^C

This example highlights the fact that rule order can be important in SEC.

The last example in this section highlights a very common use of SEC- match patterns,
keep various patterns in different contexts, and report those patterns; either ``on
demand'' as in this example, or though a context action (Section 4.1.2). The [SEC Manual Page](http://simple-evcorr.sourceforge.net/man.html)
provides an excellent example of capturing <tt class="LITERAL">FTP</tt> transactions and
reporting on those actions. See the section on the <span class="emphasis"><i class="EMPHASIS">Single*</i></span> rule.

</b></div>

<div class="SECT3">

* * *

### [4.2.3 Fill]()

<tt class="LITERAL">fill <name> [<event text>]</tt>

The <span class="emphasis"><i class="EMPHASIS">fill*</i></span> action was introduced
in SEC version 2.1.11.. This action behaves like <span class="emphasis"><i class="EMPHASIS">add*</i></span> with one important difference- the context is emptied
before the <span class="emphasis"><i class="EMPHASIS">event text*</i></span> is added.

Copy the following to <tt class="FILENAME">C4.2.03.conf</tt>:

    # Example C4.2.03.conf
    # Rule 1: create context MY_CONTEXT
    # Rule 2: report context MY_CONTEXT
    # Rule 3: 'fill' action clears context
    # Rule 4: add to context MY_CONTEXT
    #

    # Pattern 'CreateMe' creates the action
    type=Single
    ptype=RegExp
    pattern=CreateMe
    desc=$0
    action=create MY_CONTEXT

    # Pattern 'ReportMe' executes the report action
    type=Single
    ptype=RegExp
    pattern=ReportMe
    context=MY_CONTEXT
    desc=$0
    action=report MY_CONTEXT /bin/cat

    # Pattern 'FillMe' executes fill action- clears context and
    #  adds text specified
    type=Single
    ptype=RegExp
    pattern=FillMe
    desc=$0
    action=fill MY_CONTEXT  fill executed

    # Anything else gets added to context 'MY_CONTEXT'
    type=Single
    ptype=RegExp
    pattern=(.*)
    desc=$0
    action=add MY_CONTEXT $0

Run without informative debug:

    <tt class="PROMPT">%</tt> <tt class="USERINPUT">**perl sec.pl -conf=C4.2.03.conf -input=- -debug=4**</tt>

Output looks similar to:

    jpb@jpb-lt:~/SEC-examples$perl sec.pl -conf=C4.2.03.conf -input=- -debug=4
    Simple Event Correlator version 2.1.11
    Reading configuration from C4.2.03.conf
    <tt class="USERINPUT">**CreateMe**</tt>
    <tt class="USERINPUT">**line one**</tt>
    <tt class="USERINPUT">**line two**</tt>
    <tt class="USERINPUT">**ReportMe**</tt>
    line one
    line two
    <tt class="USERINPUT">**line three**</tt>
    <tt class="USERINPUT">**line four**</tt>
    <tt class="USERINPUT">**ReportMe**</tt>
    line one
    line two
    line three
    line four
    <tt class="USERINPUT">**FillMe**</tt>
    <tt class="USERINPUT">**line five**</tt>
    <tt class="USERINPUT">**line six**</tt>
    <tt class="USERINPUT">**ReportMe**</tt>
    fill executed
    line five
    line six
    ^C

As shown above, the <span class="emphasis"><i class="EMPHASIS">fill*</i></span> action
clears out the context. If the <span class="emphasis"><i class="EMPHASIS">event
text*</i></span> is not given, <span class="emphasis"><i class="EMPHASIS">%s*</i></span>
(the ``desc'' parameter) is assumed for it's value. The next matching event will be added
to the context immediately after any text included with the <span class="emphasis"><i class="EMPHASIS">fill*</i></span> action.

</div>

<div class="SECT3">

* * *

### [4.2.4 Delete]()

<tt class="LITERAL">delete <name></tt>

The <span class="emphasis"><i class="EMPHASIS">delete*</i></span> action simply deletes
the context. Happily, if the context doesn't exist no operation is performed. Just as in
perl, there is no harm in undefining a variable that doesn't exist.

To see the effect of this action, copy the following to <tt class="FILENAME">C4.2.04.conf</tt>:

    # Example C4.2.04.conf
    # Rule 1: create context MY_CONTEXT
    # Rule 2: report context MY_CONTEXT
    # Rule 3: 'delete' action deletes context
    # Rule 4: add to context MY_CONTEXT
    #

    # Pattern 'CreateMe' creates the action
    type=Single
    ptype=RegExp
    pattern=CreateMe
    desc=$0
    action=create MY_CONTEXT

    # Pattern 'ReportMe' executes the report action
    type=Single
    ptype=RegExp
    pattern=ReportMe
    context=MY_CONTEXT
    desc=$0
    action=report MY_CONTEXT /bin/cat

    # Pattern 'DeleteMe' executes delete action- deletes context
    type=Single
    ptype=RegExp
    pattern=DeleteMe
    desc=$0
    action=delete MY_CONTEXT

    # Anything else gets added to context 'MY_CONTEXT'
    type=Single
    ptype=RegExp
    pattern=(.*)
    desc=$0
    action=add MY_CONTEXT $0

Run without informative debug:

    <tt class="PROMPT">%</tt> <tt class="USERINPUT">**perl sec.pl -conf=C4.2.04.conf -input=- -debug=4**</tt>

The output should look similar to:

    pb@jpb-lt:~/SEC-examples$perl sec.pl -conf=C4.2.04.conf -input=- -debug=4
    Simple Event Correlator version 2.1.11
    Reading configuration from C4.2.04.conf
    <tt class="USERINPUT">**CreateMe**</tt>
    <tt class="USERINPUT">**line one**</tt>
    <tt class="USERINPUT">**line two**</tt>
    <tt class="USERINPUT">**line three**</tt>
    <tt class="USERINPUT">**ReportMe**</tt>
    line one
    line two
    line three
    <tt class="USERINPUT">**DeleteMe**</tt>
    <tt class="USERINPUT">**ReportMe**</tt>
    <tt class="USERINPUT">**DeleteMe**</tt>
    <tt class="USERINPUT">**ReportMe**</tt>
    ^C

</div>

<div class="SECT3">

* * *

### [4.2.5 Copy]()

<tt class="LITERAL">copy <name> %<letter></tt>

The <span class="emphasis"><i class="EMPHASIS">copy*</i></span> action takes the
contents of a context and assigns it to a SEC letter variable. In previous versions of
SEC, there was no way to access the contents of a context from inside SEC. The only way
was to run <span class="emphasis"><i class="EMPHASIS">report*</i></span>, or <span class="emphasis"><i class="EMPHASIS">shellcmd*</i></span> actions which feed context
content to external programs.

This can be useful for determining current state in a complex set of events. Suppose
that there were several motors used in an experiment, each one sending state information
such as ``<tt class="LITERAL">ON</tt>'' ``<tt class="LITERAL">OFF</tt>'', ``<tt class="LITERAL">SLOWSPEED</tt>'', ``<tt class="LITERAL">FASTSPEED</tt>'', etc. to a
central console. As the experiment proceeds and the motors change state, it is now
possible to get the current state (and all previous states) into a single variable.

</div>
</div>

<div class="SECT2">

* * *

## [4.3 Perl
Mini-Programs]()

Introduced in Section 4, perl mini-programs are a feature designed to provide
additional flexibility in rule matching. Instead of a checking for a fixed context name
or being concerned with context content, perl mini-programs are evaluated by the perl <b class="APPLICATION">eval** command and the results returned to SEC. A non-zero return
results in the context being TRUE, and any action statement is executed.

The mini-program feature allows an external 'program' to determine whether an matched
string should be processed. Consider the following example:

    # Example C4.3.01.conf
    # Mini-program examples
    # Use mini-program to determine if matching string should
    # be processed.
    # This mini-program returns true if a randomly generated
    # number is between 5 and 9.
    #

    type=Single
    ptype=RegExp
    pattern=bar
    desc=$0
    context= =({(int rand(10) >= 5)})
    action=write - mini-program was true this time

Copy the above to <tt class="FILENAME">C4.3.01.conf</tt>.

Run this example with:

    <tt class="PROMPT">%</tt> <tt class="USERINPUT">**perl sec.pl -conf=C4.3.01.conf -input=-**</tt>

Output will look similar to:

    jpb@jpb-lt:~/SEC-examples$perl sec.pl -conf=C4.3.01.conf -input=-
    Simple Event Correlator version 2.1.11
    Reading configuration from C4.3.01.conf
    1 rules loaded from C4.3.01.conf
    <tt class="USERINPUT">**bar**</tt>
    Writing event 'mini-program was true this time' to file -
    mini-program was true this time
    <tt class="USERINPUT">**bar**</tt>
    <tt class="USERINPUT">**bar**</tt>
    Writing event 'mini-program was true this time' to file -
    mini-program was true this time
    <tt class="USERINPUT">**bar**</tt>
    <tt class="USERINPUT">**bar**</tt>
    <tt class="USERINPUT">**bar**</tt>
    Writing event 'mini-program was true this time' to file -
    mini-program was true this time
    <tt class="USERINPUT">**bar**</tt>
    Writing event 'mini-program was true this time' to file -
    mini-program was true this time
    <tt class="USERINPUT">**bar**</tt>
    <tt class="USERINPUT">**bar**</tt>
    <tt class="USERINPUT">**bar**</tt>
    <tt class="USERINPUT">**bar**</tt>
    Writing event 'mini-program was true this time' to file -
    mini-program was true this time
    <tt class="USERINPUT">**bar**</tt>
    Writing event 'mini-program was true this time' to file -
    mini-program was true this time
    <tt class="USERINPUT">**bar**</tt>
    <tt class="USERINPUT">**bar**</tt>
    ^C

</b></div>

<div class="SECT2">

* * *

## [4.4
Calendar Rules and Contexts]()

The <span class="emphasis"><i class="EMPHASIS">calendar*</i></span> rule (discussed
previously in Section 3.2.8) is similar to the Unix <b class="APPLICATION">cron**
utility. Calendar rules are often used with contexts. A frequent question is how to set
up SEC to understand the notion of ``Business Hours'' and ``Off Hours''. One solution
might be:

    type=Calendar 
    time=0 6 * * * 
    desc=BUSINESS_HOURS
    action=create %s 54000 (create OFF_HOURS 32400)

This solution creates context BUSINESS_HOURS at 6:00am every morning that lasts for
fifteen hours (54000 seconds), they creates context OFF_HOURS which lasts for nine hours
(32400 seconds). Unfortunately, this solution doesn't take weekends into account, and
more importantly, it only works if SEC happens to be running at precisely 6:00am.

A more robust solution actually takes three rules- one to set BUSINESS_HOURS every
minute between 6:00am - 8:59pm, M-F; and set OFF_HOURS 9:00pm - 5:59am M-F and all day
Saturday and Sunday:

    # Business Hours- applies 6:00am - 8:59pm, M-F
    # Off hours -  9:00pm - 5:59am M-F and all day Sat and Sun.
    # Set every minute by the calendar rules below
    # See  crontab(5)
    #
    # Three calendars are needed since calendar is run every minute
    # and we don't actually know when SEC will be started (or restarted)
    #
    #
    # Calendar Rule 1- set BUSINESS_HOURS
    type=Calendar
    time=* 6-20 * * 1,2,3,4,5
    desc=BUSINESS_HOURS
    context=!BUSINESS_HOURS
    action=create %s;\
           write - Switched to Business Hours;\
           delete OFF_HOURS;

    # Calendar Rule 2- set OFF_HOURS during M-F
    type=Calendar
    time=* 0-5,21-23 * * *
    context=!OFF_HOURS
    desc=OFF_HOURS
    action=create %s;\
           write - Switched to Off Hours;\
           delete BUSINESS_HOURS;

    # Calendar Rule 3- set OFF_HOURS during Sat,Sun
    type=Calendar
    time=* * * * 6,7
    context=!OFF_HOURS
    desc=OFF_HOURS
    action=create %s;\
           write - Switched to Off Hours;\
           delete BUSINESS_HOURS;

On startup, this rule set will write to standard out:

    jpb@jpb-lt:~/SEC-examples$perl sec.pl -conf=C4.4.01.conf -input=- -debug=4
    Simple Event Correlator version 2.1.11
    Reading configuration from C4.4.01.conf
    Switched to Business Hours
    ^C

At the appropriate time SEC will shuffle it's contexts and write ``<tt class="LITERAL">Switched to Off Hours</tt>'' or ``<tt class="LITERAL">Switched to
Business Hours</tt>'' to standard output.

</b></div>
</div>

<div class="SECT1">

* * *

# [5 Applications to Logfile Monitoring]()

SEC excels at logfile monitoring. With an appropriate set of rules, SEC can be
configured to monitor logfiles for a single host, a small group of systems, or a whole
enterprise. And since rules are simply text files, any time a new rule is needed it can
be quickly developed and added to SEC on the fly.

This section begins with some simple logging entries, examines some typical problems
and solutions, then explores issues of robustness, and scalability. The configurations in
this section have been developed for BSD systems, but the basic principles can be applied
to any Unix system. In Part Two of this article, there is also a section on monitoring
Windows security event logs.

<div class="IMPORTANT">

> **Important:** Before beginning to monitor logfiles, first make sure you are
> allowed to do so! If you are using your own system, or are already the system
> administrator for your system, fine. Otherwise, obtain permission from your system
> administrator before mucking around in logfiles.

</div>

<div class="SECT2">

* * *

## [5.1
Logfile Monitoring Examples]()

To begin, login as a normal user and deliberately fail a privilege escalation to
root:

    <tt class="PROMPT">%</tt> <tt class="USERINPUT">**su -**</tt> 
    Password: (deliberately type a wrong password)

Next, examine your logfile (<tt class="FILENAME">/var/log/messages</tt> on most BSD
systems) for the logged notification:

    Nov 18 09:37:38 jpb-lt su: BAD SU jpb to root on /dev/ttyp3

A SEC rule to pick up this logfile entry is:

    #   Bad su 
    # ----------- 
    #
    type=Single
    ptype=RegExp 
    desc=$0 
    pattern=\S+\s+\d+\s+\S+\s+(\S+)\s+su: BAD SU (\S+) to (\S+) on (\S+)
    action=write - $2 failed SU to $3 on $1 at %t

Save the above rule as <tt class="FILENAME">L01.conf</tt> and run with:

    <tt class="PROMPT">%</tt> <tt class="USERINPUT">**perl sec.pl -conf=L01.conf -input=/var/log/messages**</tt>

Leave the above command running in one screen, and in another screen or terminal
session, try to deliberately fail an <b class="APPLICATION">su** command as noted
above.

Output should be similar to:

    jpb@jpb-lt:~/SEC-examples$perl sec.pl -conf=L01.conf -input=/var/log/messages
    Simple Event Correlator version 2.1.11
    Reading configuration from L01.conf
    1 rules loaded from L01.conf
    Writing event 'jpb failed SU to root on jpb-lt at Tue Nov 18 10:21:36 2003' to file -
    jpb failed SU to root on jpb-lt at Tue Nov 18 10:21:36 2003

SEC has successfully detected a bad <b class="APPLICATION">su** to root.

Depending on your level of paranoia, you may decide that you can use a <span class="emphasis"><i class="EMPHASIS">SingleWithThreshold*</i></span> rule to allow for
some fat on the fingers.

<div class="SECT3">

* * *

### [5.1.1 Monitoring SEC Notifications]()

There are two problems with the above examples. The first is that by using the <span class="emphasis"><i class="EMPHASIS">write*</i></span> action, all we have done is
replaced one logging mechanism with another. Using <span class="emphasis"><i class="EMPHASIS">write*</i></span> to generate a new logfile, or even to write to standard
output, is equivalent to syslog. We haven't gained anything.

A partial solution is to move to an 'interrupt' method of notification. In the above
example, the <span class="emphasis"><i class="EMPHASIS">write*</i></span> action can be
replaced with an action to email the system administrator using the <span class="emphasis"><i class="EMPHASIS">pipe*</i></span> action:

    action=pipe '$2 failed SU to $3 on $1 at %t' /usr/bin/mailx -s "LoginFailure" root@localhost

Output now looks like:

    jpb@jpb-lt:~/SEC-examples$perl sec.pl -conf=L01.conf -input=/var/log/messages
    Simple Event Correlator version 2.1.11
    Reading configuration from L01.conf
    1 rules loaded from L01.conf
    Feeding event 'jpb failed SU to root on jpb-lt at Sat Nov 22 13:24:16 2003' to shell command '/usr/bin/mailx -s "LoginFailure" root@localhost'
    Child 1053 created for command '/usr/bin/mailx -s "LoginFailure" root@localhost'

The email message contains the completed text with all backreferences filled in:

    Date: Sat, 22 Nov 2003 13:24:16 -0500 (EST)
    From: "Jim B." <jpb@jpb-lt>
    To: root@jpb-lt
    Subject: LoginFailure

    jpb failed SU to root on jpb-lt at Sat Nov 22 13:24:16 2003

The second problem is that this doesn't scale well. There is no aggregation of event
data. It's quite common for email and paging notification systems to be turned off in
large organizations because there are simply too many notifications- too many pages and
emails. Unless there is some sort of aggregation and/or filtering mechanism, pages and
emails are likely to be ignored.

A solution to this problem is presented in Part Two of this article.

</div>
</b></b></div>

<div class="SECT2">

* * *

## [5.2 Using SEC with Syslog on BSD Systems]()

In order to use SEC to process <b class="APPLICATION">syslog** messages, we first
need to decide which messages we want to monitor. Below is a small collection of syslog
messages organized into various categories.

This collection contains entries that are generally <span class="emphasis"><i class="EMPHASIS">security related*</i></span>. There are many other syslog entries in the
BSD kernel, system, and user programs. A more comprehensive collection will be used in
Part Two of this article,

<div class="SECT3">

* * *

### [5.2.1 MONITOR Rules]()

The MONITOR group of syslog messages concerns syslogd itself, and messages that infer
that promiscuous mode has been enabled. The syslog messages for the MONITOR group
are:

    
Logs involving syslogd disabled or unusual promiscuous mode (MONITOR)
    ----------------------------------------------------------------------
    Nov 15 20:02:48 foohost syslogd: exiting on signal 15
    Nov 22 02:00:02 foohost syslogd: restart
    Nov 11 15:58:55 foohost /kernel: de0: promiscuous mode enabled
    Nov 11 15:58:57 foohost /kernel: de0: promiscuous mode disabled

SEC rules for the MONITOR group are as follows:

    # MONITOR.conf - SEC rules to pick up disruptive monitoring
    # events.
    #
    #Logs involving syslogd disabled or unusual promiscuous mode (MONITOR)
    #----------------------------------------------------------------------
    #Nov 15 20:02:48 foohost syslogd: exiting on signal 15
    #Nov 22 02:00:02 foohost syslogd: restart
    #Nov 11 15:58:55 foohost /kernel: de0: promiscuous mode enabled
    #Nov 11 15:58:57 foohost /kernel: de0: promiscuous mode disabled
    #

    #
    # Syslog Exit
    # -----------
    #
    type=Single
    ptype=RegExp
    pattern=\S+\s+\d+\s+\S+\s+(\S+)\s+syslogd: exiting on signal (\d+)
    desc=$0
    action=write - MONITOR: $1 syslog exit on signal $2 at %t

    #
    # Syslog Restart
    # ---------------
    #
    type=Single
    ptype=RegExp
    pattern=\S+\s+\d+\s+\S+\s+(\S+)\s+syslogd: restart
    desc=$0
    action=write - MONITOR: $1 syslog restart at %t

    #
    # Syslog Exit
    # -----------
    #
    type=Single
    ptype=RegExp
    pattern=\S+\s+\d+\s+\S+\s+(\S+)\s+/kernel: (\S+) promiscuous mode (\S+)
    desc=$0
    action=write - MONITOR: $1 $2 promiscuous mode $3 at %t

Copy the above syslog entries to <tt class="FILENAME">MONITOR.txt</tt>.

</div>

<div class="SECT3">

* * *

### [5.2.2 PHYSMOD Rules]()

The PHYSMOD group concerns messages that infer that physical modifications have been
performed on the host such as a new PCMCIA card, or that a cable has been disconnected.
The syslog messages for the PHYSMOD group are:

    
Logs involving physical modifications (PHYSMOD)
    ------------------------------------------------
    Nov 14 21:11:19 foohost /kernel: pccard: card inserted, slot 0
    Nov 14 22:28:09 foohost /kernel: pccard: card removed, slot 0
    Nov 12 19:46:31 foohost /kernel: de0: link down: cable problem?
    Nov 12 19:46:42 foohost /kernel: de0: autosense failed: cable problem?
    Oct 18 06:26:37 foohost pccardd[49]: ep0: 3Com Corporation (/3C589/) inserted.
    Oct 18 06:26:42 foohost pccardd[49]: pccardd started

Copy the above syslog entries to <tt class="FILENAME">PHYSMOD.txt</tt>.

SEC rules for the PHYSMOD group are:

    # PHYSMOD.conf - Events concerning physical modifications
    #                to the system.
    #
    #
    #Logs involving physical modifications (PHYSMOD)
    #------------------------------------------------
    #Nov 14 21:11:19 foohost /kernel: pccard: card inserted, slot 0
    #Nov 14 22:28:09 foohost /kernel: pccard: card removed, slot 0
    #Nov 12 19:46:31 foohost /kernel: de0: link down: cable problem?
    #Nov 12 19:46:42 foohost /kernel: de0: autosense failed: cable problem?
    #Oct 18 06:26:37 foohost pccardd[49]: ep0: 3Com Corporation (/3C589/) inserted.
    #Oct 18 06:26:42 foohost pccardd[49]: pccardd started
    #

    #
    # PCMCIA Card Insertion, Removal
    # --------------------------------
    #
    type=Single
    ptype=RegExp
    pattern=\S+\s+\d+\s+\S+\s+(\S+)\s+/kernel: pccard: card (\S+), slot (\d+)
    desc=$0
    action=write - PHYSMOD: $1 pccard: card $2 in slot $3 at %t

    #
    # PCMCIA Card Daemon
    # --------------------------------
    #
    type=Single
    ptype=RegExp
    pattern=\S+\s+\d+\s+\S+\s+(\S+)\s+pccardd\[\d+\]: (.*)
    desc=$0
    action=write - PHYSMOD: $1 pccardd: $2 at %t

    #
    # Cabling Problem
    # ----------------
    #
    type=Single
    ptype=RegExp
    pattern=\S+\s+\d+\s+\S+\s+(\S+)\s+/kernel: (\S+)\s+(.*?:) cable problem
    desc=$0
    action=write - PHYSMOD: $1 cable problem on $2, text: $3 at %t

</div>

<div class="SECT3">

* * *

### [5.2.3 USERACT Rules]()

The USERACT group concerns messages that are relevant to user activity, such as login
failures or change of userid. The syslog messages for the USERACT group are:

    
Logs involving logins, change of UID and privilege escalations (USERACT)
    -------------------------------------------------------------------------
    Nov 14 12:14:58 foohost sshd[3388]: fatal: Timeout before authentication for 192.168.1.1
    Nov 14 19:58:34 foohost sshd[6597]: Bad protocol version identification '^B^S^D^Q^L' from 192.168.1.100
    Nov 15 04:02:24 foohost login: 1 LOGIN FAILURE ON ttyp2
    Nov 15 04:02:24 foohost login: 1 LOGIN FAILURE ON ttyp2, mysql
    Oct 18 02:52:04 foohost login: ROOT LOGIN (root) ON ttyv1
    Oct 18 03:20:46 foohost login: 2 LOGIN FAILURES ON ttyv0
    Nov 10 19:40:03 foohost su: jpb to root on /dev/ttyp0
    Nov 18 09:37:38 foohost su: BAD SU jpb to root on /dev/ttyp3
    Nov 22 12:26:44 foohost su: BAD SU badboy to root on /dev/ttyp0
    Oct 18 06:11:11 foohost login: login on ttyv0 as root
    Oct 18 06:16:53 foohost sshd[131]: Accepted keyboard-interactive/pam for jpb from 192.168.1.1 port 1077 ssh2
    Nov 14 12:55:29 foohost sshd[3425]: Accepted keyboard-interactive/pam for jpb from fe80::2c0:4fff:fe18:13fd%ep0 port 27492 ssh2

Copy the above syslog entries to <tt class="FILENAME">USERACT.txt</tt>.

SEC rules for the USERACT group are:

    # USERACT.conf - Events concerning user activities.
    #
    #Logs involving logins, change of UID and privilege escalations (USERACT)
    #-------------------------------------------------------------------------
    #Nov 14 12:14:58 foohost sshd[3388]: fatal: Timeout before authentication for 192.168.1.1
    #Nov 14 19:58:34 foohost sshd[6597]: Bad protocol version identification '^B^S^D^Q^L' from 192.168.1.100
    #Oct 18 06:16:53 foohost sshd[131]: Accepted keyboard-interactive/pam for jpb from 192.168.1.1 port 1077 ssh2
    #Nov 14 12:55:29 foohost sshd[3425]: Accepted keyboard-interactive/pam for jpb from fe80::2c0:4fff:fe18:13fd%ep0 port 27492 ssh2
    #Nov 15 04:02:24 foohost login: 1 LOGIN FAILURE ON ttyp2
    #Nov 15 04:02:24 foohost login: 1 LOGIN FAILURE ON ttyp2, mysql
    #Oct 18 03:20:46 foohost login: 2 LOGIN FAILURES ON ttyv0
    #Oct 18 02:52:04 foohost login: ROOT LOGIN (root) ON ttyv1
    #Oct 18 06:11:11 foohost login: login on ttyv0 as root
    #Nov 10 19:40:03 foohost su: jpb to root on /dev/ttyp0
    #Nov 18 09:37:38 foohost su: BAD SU jpb to root on /dev/ttyp3
    #Nov 22 12:26:44 foohost su: BAD SU badboy to root on /dev/ttyp0
    #

    #
    # sshd Problems
    # --------------
    #
    type=Single
    ptype=RegExp
    pattern=\S+\s+\d+\s+\S+\s+(\S+)\s+sshd\[\d+\]: (fatal|Bad)(.*)
    desc=$0
    action=write - USERACT: $1 sshd $2 problem, text: $3 at %t

    #
    # sshd Accepted
    # --------------
    #
    type=Single
    ptype=RegExp
    pattern=\S+\s+\d+\s+\S+\s+(\S+)\s+sshd\[\d+\]: Accepted (.*)
    desc=$0
    action=write - USERACT: $1 sshd accepted login, text: $2 at %t

    #
    # login FAILURES
    # ---------------
    #
    type=Single
    ptype=RegExp
    pattern=\S+\s+\d+\s+\S+\s+(\S+)\s+login: (.*?FAILURE.)(.*?ON) (.*)
    desc=$0
    action=write - USERACT: $1 login $2 on $4 at %t

    #
    # su  bad
    # -----------------
    #
    type=Single
    ptype=RegExp
    pattern=\S+\s+\d+\s+\S+\s+(\S+)\s+su: (BAD SU) (\S+) to (\S+) on (\S+)
    desc=$0
    action=write - USERACT: $1 su: $2 $3 to $4 on $5 at %t

    #Nov 10 19:40:03 foohost su: jpb to root on /dev/ttyp0
    #Nov 18 09:37:38 foohost su: BAD SU jpb to root on /dev/ttyp3
    #Nov 22 12:26:44 foohost su: BAD SU badboy to root on /dev/ttyp0
    #
    #
    # su  good to root
    # -----------------
    #
    type=Single
    ptype=RegExp
    pattern=\S+\s+\d+\s+\S+\s+(\S+)\s+su: (\S+) to root on (\S+)
    desc=$0
    action=write - USERACT: $1 su: $2 to ROOT on $4  at %t

</div>
</b></div>

<div class="SECT2">

* * *

## [5.3 Testing The Rule
Set]()

Testing the SEC rule set for the above three groups of rules is accomplished by
running SEC with all three configuration files and dumping the collection of syslog
entries into SEC's input.

Use the following command line to test the rule sets:

    <tt class="PROMPT">%</tt> <tt class="USERINPUT">**perl sec.pl -conf=MONITOR.conf \
            -conf=PHYSMOD.conf \
            -conf=USERACT.conf \
            -input=SEC.test \
            -debug=4**</tt>

This command sets up <tt class="FILENAME">SEC.test</tt> as SEC's input file. Create an
empty <tt class="FILENAME">SEC.test</tt> and run the above command. Then, in a separate
window or terminal session, perform the following command:

    <tt class="PROMPT">%</tt> <tt class="USERINPUT">**cat MONITOR.txt PHYSMOD.txt USERACT.txt >> SEC.test**</tt>

Check SEC's output against the rules for any missing matches on input. The results
will look similar to:

    jpb@jpb-lt:~/SEC-examples$perl sec.pl -conf=MONITOR.conf \
    >                 -conf=PHYSMOD.conf \
    >                 -conf=USERACT.conf \
    >                 -input=SEC.test \
    >                 -debug=4
    Simple Event Correlator version 2.1.11
    Reading configuration from MONITOR.conf
    Reading configuration from PHYSMOD.conf
    Reading configuration from USERACT.conf
    MONITOR: foohost syslog exit on signal 15 at Sat Nov 22 17:44:07 2003
    MONITOR: foohost syslog restart at Sat Nov 22 17:44:07 2003
    MONITOR: foohost de0: promiscuous mode enabled at Sat Nov 22 17:44:07 2003
    MONITOR: foohost de0: promiscuous mode disabled at Sat Nov 22 17:44:07 2003
    PHYSMOD: foohost pccard: card inserted in slot 0 at Sat Nov 22 17:44:07 2003
    PHYSMOD: foohost pccard: card removed in slot 0 at Sat Nov 22 17:44:07 2003
    PHYSMOD: foohost cable problem on de0:, text: link down: at Sat Nov 22 17:44:07 2003
    PHYSMOD: foohost cable problem on de0:, text: autosense failed: at Sat Nov 22 17:44:07 2003
    PHYSMOD: foohost pccardd: ep0: 3Com Corporation (/3C589/) inserted. at Sat Nov 22 17:44:07 2003
    PHYSMOD: foohost pccardd: pccardd started at Sat Nov 22 17:44:07 2003
    USERACT: foohost sshd fatal problem, text: : Timeout before authentication for 192.168.1.1 at Sat Nov 22 17:44:07 2003
    USERACT: foohost sshd Bad problem, text:  protocol version identification '^B^S^D^Q^L' from 192.168.1.100 at Sat Nov 22 17:44:07 2003
    USERACT: foohost login 1 LOGIN FAILURE  on ttyp2 at Sat Nov 22 17:44:07 2003
    USERACT: foohost login 1 LOGIN FAILURE  on ttyp2, mysql at Sat Nov 22 17:44:07 2003
    USERACT: foohost login 2 LOGIN FAILURES on ttyv0 at Sat Nov 22 17:44:07 2003
    USERACT: foohost su: jpb to ROOT on $4  at Sat Nov 22 17:44:07 2003
    USERACT: foohost su: BAD SU jpb to root on /dev/ttyp3 at Sat Nov 22 17:44:07 2003
    USERACT: foohost su: BAD SU badboy to root on /dev/ttyp0 at Sat Nov 22 17:44:07 2003
    USERACT: foohost sshd accepted login, text: keyboard-interactive/pam for jpb from 192.168.1.1 port 1077 ssh2 at Sat Nov 22 17:44:07 2003
    USERACT: foohost sshd accepted login, text: keyboard-interactive/pam for jpb from fe80::2c0:4fff:fe18:13fd%ep0 port 27492 ssh2 at Sat Nov 22 17:44:07 2003
    ^C

Once you are satisfied, run the following command to monitor syslog message on your
BSD logfile, <tt class="FILENAME">/var/log/messages</tt>:

    <tt class="PROMPT">%</tt> <tt class="USERINPUT">**perl sec.pl -conf=MONITOR.conf \
            -conf=PHYSMOD.conf \
            -conf=USERACT.conf \
            -input=/var/log/messages \
            -debug=4**</tt>

Until we have a more comprehensive approach to correlating events you may wish to keep
the action statements limited to simple <span class="emphasis"><i class="EMPHASIS">write*</i></span> actions shown in the above rules. If you do decide to
use email or paging for your actions (as shown in Section 5.1.1), test on a small subset
of rules first.

Use the <span class="emphasis"><i class="EMPHASIS">-detach*</i></span> to put the SEC
monitoring application in the background:

    <tt class="PROMPT">%</tt> <tt class="USERINPUT">**perl sec.pl -conf=MONITOR.conf \
            -conf=PHYSMOD.conf \
            -conf=USERACT.conf \
            -input=/var/log/messages \
            -debug=4  \
            -detach**</tt>

</div>
</div>

<div class="SECT1">

* * *

# [6 Part One
Conclusion]()

In this article, we've looked at <b class="APPLICATION">SEC**, the Simple Event
Correlator, from Risto Vaarandi. Using <b class="APPLICATION">perl** regular expression
patterns along with SEC rule types and actions, we have examined how SEC works, and how
to create and use rule sets for logfile monitoring.

</b></b></div>
