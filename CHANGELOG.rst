================
Xonsh Change Log
================

Current Developments
====================

**Added:**

* Tab completers can now raise ``StopIteration`` to prevent consideration of
  remaining completers.
* Added tab completer for the ``completer`` alias.
* New ``Block`` and ``Functor`` context managers are now available as
  part of the ``xonsh.contexts`` module.
* ``Block`` provides support for turning a context body into a non-executing
  list of string lines. This is implmement via a syntax tree transformation.
  This is useful for creating remote execution tools that seek to prevent
  local execution.
* ``Functor`` is a subclass of the ``Block`` context manager that turns the
  block into a callable object.  The function object is available via the
  ``func()`` attribute.  However, the ``Functor`` instance is itself callable
  and will dispatch to ``func()``.
* New ``$VC_BRANCH_TIMEOUT`` environment variable is the time (in seconds)
  of how long to spend attempting each individual version control branch
  information command during ``$PROMPT`` formatting.  This allows for faster
  prompt resolution and faster startup times.
* New lazy methods added to CommandsCache allowing for testing and inspection
  without the possibility of recomputing the cache.
* ``!(command)`` is now usefully iterable, yielding lines of stdout
* Added XonshCalledProcessError, which includes the relevant CompletedCommand.
  Also handles differences between Py3.4 and 3.5 in CalledProcessError
* Tab completion of paths now includes zsh-style path expansion (subsequence
  matching), toggleable with ``$SUBSEQUENCE_PATH_COMPLETION``
* Tab completion of paths now includes "fuzzy" matches that are accurate to
  within a few characters, toggleable with ``$FUZZY_PATH_COMPLETION``
* Provide ``$XONSH_SOURCE`` for scripts in the environment variables pointing to
  the currently running script's path
* Arguments '+' and '-' for the ``fg`` command (job control)
* Provide ``$XONSH_SOURCE`` for scripts in the environment variables pointing to
  the currently running script's path
* ``!(command)`` is now usefully iterable, yielding lines of stdout
* Added XonshCalledProcessError, which includes the relevant CompletedCommand.
  Also handles differences between Py3.4 and 3.5 in CalledProcessError
* XonshError and XonshCalledProcessError are now in builtins

**Changed:**

* Functions in ``Execer`` now take ``transform`` kwarg instead of
  ``wrap_subproc``.
* Provide ``$XONSH_SOURCE`` for scripts in the environment variables pointing to
  the currently running script's path
* XonshError and XonshCalledProcessError are now in builtins

**Deprecated:** None

**Removed:**

* ``ensure_git()`` and ``ensure_hg()`` decorators removed.
* ``call_hg_command()`` function removed.

**Fixed:**

* Strip leading space in commands passed using the "-c" switch
* Fixed xonfig wizard failing on Windows due to colon in created filename.
* Ensured that the prompt_toolkit shell functions, even without a ``completer``
  attribute.
* Fixed crash resulting from malformed ``$PROMPT`` or ``$TITLE``.
* xonsh no longer backgrounds itself after every command on Cygwin.
* Fixed an issue about ``os.killpg()`` on Cygwin which caused xonsh to crash
  occasionally
* Fix crash on startup when Bash Windows Subsystem for Linux is on the Path. 

**Security:** None

v0.3.4
====================

**Changed:**

* ``$PROMPT`` from foreign shells is now ignored.
* ``$RC_FILES`` environment variable now stores the run control files we
  attempted to load.
* Only show the prompt for the wizard if we did not attempt to load any run
  control files (as opposed to if none were successfully loaded).
* Git and mercurial branch and dirty function refactor to imporve run times.


**Fixed:**

* Fixed an issue whereby attempting to delete a literal value (e.g., ``del 7``)
  in the prompt_toolkit shell would cause xonsh to crash.
* Fixed broken behavior when using ``cd ..`` to move into a nonexistent
  directory.
* Partial workaround for Cygwin where ``pthread_sigmask`` appears to be missing
  from the ``signal`` module.
* Fixed crash resulting from malformed ``$PROMPT``.
* Fixed regression on Windows with the locate_binary() function.
  The bug prevented `source-cmd` from working correctly and broke the
  ``activate``/``deactivate`` aliases for the conda environements.
* Fixed crash resulting from errors other than syntax errors in run control
  file.


v0.3.3
====================
**Added:**

* Question mark literals, ``?``, are now allowed as part of
  subprocess argument names.
* IPython style visual pointer to show where syntax error was detected
* Pretty printing of output and syntax highlighting of input and output can now
  be controlled via new environment variables ``$COLOR_INPUT``,
  ``$COLOR_RESULTS``, and ``$PRETTY_PRINT_RESULTS``.

* In interactive mode, if there are stopped or background jobs, Xonsh prompts
  for confirmations rather than just killing all jobs and exiting.

**Changed:**

* ``which`` now gives a better verbose report of where the executables are
  found.
* Tab completion now uses a different interface, which allows new completers
  to be implemented in Python.
* Most functions in the ``Execer`` now take an extra argument
  ``transform``, indicating whether the syntax tree transformations should
  be applied.
* ``prompt_toolkit`` is now loaded lazily, decreasing load times when using
  the ``readline`` shell.
* RC files are now executed directly in the appropriate context.
* ``_`` is now updated by ``![]``, to contain the appropriate
  ``CompletedCommand`` object.
* On Windows if bash is not on the path look in the registry for the defaults
  install directory for GitForWindows.



**Removed:**

* Fixed bug on Windows where ``which`` did not include current directory

**Fixed:**

* Fixed crashed bash-completer when bash is not avaiable on Windows
* Fixed bug on Windows where tab-completion for executables would return all files.
* Fixed bug on Windows which caused the bash $PROMPT variable to be used when no
  no $PROMPT variable was set in .xonshrc
* Improved start-up times by caching information about bash completion
  functions
* The --shell-type CLI flag now takes precedence over $SHELL_TYPE specified in
  .xonshrc
* Fixed an issue about ``os.killpg()`` on OS X which caused xonsh crash with
  occasionality



v0.3.2
====================
**Fixed:**

* Fixed PermissionError when tab completions tries to lookup executables in
  directories without read permissions.
* Fixed incorrect parsing of command line flags



v0.3.1
====================
**Added:**

* When a subprocess exits with a signal (e.g. SIGSEGV), a message is printed,
  similar to Bash.
* Added comma literals to subproc mode.
* ``@$(cmd)`` has been added as a subprocess-mode operator, which replaces in
  the subprocess command itself with the result of running ``cmd``.
* New ``showcmd`` alias for displaying how xonsh interprets subprocess mode
  commands and arguments.
* Added ``$DYNAMIC_CWD_WIDTH`` to allow the adjusting of the current working
  directory width in the prompt.
* Added ``$XONSH_DEBUG`` environment variable to help with debuging.
* The ``${...}`` shortcut for ``__xonsh_env__`` now returns appropriate
  completion options

**Changed:**

* On Windows the default bash completions files ``$BASH_COMPLETIONS`` now points
  to the default location of the completions files used by 'Git for Windows'
* On Cygwin, some tweaks are applied to foreign shell subprocess calls and the
  readline import, in order to avoid hangs on launch.


**Removed:**

* Special cased code for handling version of prompt_toolkit < v1.0.0


**Fixed:**

* Show sorted bash completions suggestions.
* Fix bash completions (e.g git etc.) on windows when completions files have
  spaces in their path names
* Fixed a bug preventing ``source-bash`` from working on Windows
* Numerous improvements to job control via a nearly-complete rewrite.
* Addressed issue with finding the next break in subproc mode in context
  sensitive parsing.
* Fixed issue with loading readline init files (inputrc) that seems to be
  triggered by libedit.
* ``$MULTILINE_PROMPT`` now allows colors, as originally intended.
* Rectified install issue with Jupyter hook when installing with pyenv,
  Jupyter install hook now repects ``--prefix`` argument.
* Fixed issue with the xonsh.ply subpackage not being installed.
* Fixed a parsing bug whereby a trailing ``&`` on a line was being ignored
  (processes were unable to be started in the background)



v0.3.0
====================
**Added:**

* ``and``, ``or``, ``&&``, ``||`` have been added as subprocess logical operators,
  by popular demand!
* Subprocesses may be negated with ``not`` and grouped together with parentheses.
* New framework for writing xonsh extentions, called ``xontribs``.
* Added a new shell type ``'none'``, used to avoid importing ``readline`` or
  ``prompt_toolkit`` when running scripts or running a single command.
* New: `sudo` functionality on Windows through an alias
* Automatically enhance colors for readability in the default terminal (cmd.exe)
  on Windows. This functionality can be enabled/disabled with the
  $INTENSIFY_COLORS_ON_WIN environment variable.
* Added ``Ellipsis`` lookup to ``__xonsh_env__`` to allow environment variable checks, e.g. ``'HOME' in ${...}``
* Added an option to update ``os.environ`` every time the xonsh environment changes.
  This is disabled by default but can be enabled by setting ``$UPDATE_OS_ENVIRON`` to
  True.
* Added Windows 'cmd.exe' as a foreign shell. This gives xonsh the ability to source
  Windows Batch files (.bat and .cmd). Calling ``source-cmd script.bat`` or the
  alias ``source-bat script.bat`` will call the bat file and changes to the
  environment variables will be reflected in xonsh.
* Added an alias for the conda environment activate/deactivate batch scripts when
  running the Anaconda python distribution on Windows.
* Added a menu entry to launch xonsh when installing xonsh from a conda package
* Added a new ``which`` alias that supports both regular ``which`` and also searches
  through xonsh aliases. A pure python implementation of ``which`` is used. Thanks
  to Trent Mick. https://github.com/trentm/which/
* Added support for prompt toolkit v1.0.0.
* Added ``$XONSH_CACHE_SCRIPTS`` and ``$XONSH_CACHE_EVERYTHING`` environment
  variables to control caching of scripts and interactive commands.  These can
  also be controlled by command line options ``--no-script-cache`` and
  ``--cache-everything`` when starting xonsh.
* Added a workaround to allow ctrl-c to interrupt reverse incremental search in
  the readline shell

**Changed:**

* Running scripts through xonsh (or running a single command with ``-c``) no
  longer runs the user's rc file, unless the ``--login`` option is specified.
  Also avoids loading aliases and environments from foreign shells, as well as
  loading bash completions.
* rc files are now compiled and cached, to avoid re-parsing when they haven't
  changed.  Scripts are also compiled and cached, and there is the option to
  cache interactive commands.
* Left and Right arrows in the ``prompt_toolkit`` shell now wrap in multiline
  environments
* Regexpath matching with backticks, now returns an empty list in python mode.
* Pygments added as a dependency for the conda package
* Foreign shells now allow for setting exit-on-error commands before and after
  all other commands via the ``seterrprevcmd`` and ``seterrpostcmd`` arguments.
  Sensinble defaults are provided for existing shells.
* PLY is no longer a external dependency but is bundled in xonsh/ply. Xonsh can
  therefore run without any external dependencies, although having prompt-toolkit
  recommended.
* Provide better user feedback when running ``which`` in a platform that doesn't
  provide it (e.g. Windows).
* The lexer now uses a custom tokenizer that handles regex globs in the proper
  way.






**Fixed:**

* Fixed bug with loading prompt-toolkit shell < v0.57.
* Fixed bug with prompt-toolkit completion when the cursor is not at the end of
  the line.
* Aliases will now evaluate enviornment variables and other expansions
  at execution time rather than passing through a literal string.
* Fixed environment variables from os.environ not beeing loaded when a running
  a script
* The readline shell will now load the inputrc files.
* Fixed bug that prevented `source-alias` from working.
* Now able to ``^C`` the xonfig wizard on start up.
* Fixed deadlock on Windows when runing subprocess that generates enough output
  to fill the OS pipe buffer.
* Sourcing foreign shells will now return a non-zero exit code if the
  source operation failed for some reason.
* Fixed PermissionError when running commands in directories without read permissions
* Prevent Windows fixups from overriding environment vars in static config
* Fixed Optional Github project status to reflect added/removed files via git_dirty_working_directory()
* Fixed xonsh.exe launcher on Windows, when Python install directory has a space in it
* Fixed `$CDPATH` to support `~` and environments variables in its items




v0.2.7
====================
**Added:**

* Added new valid ``$SHELL_TYPE`` called ``'best'``. This selects the best value
  for the concrete shell type based on the availability on the user's machine.
* New environment variable ``$XONSH_COLOR_STYLE`` will set the color mapping
  for all of xonsh.
* New ``XonshStyle`` pygments style will determine the approriate color
  mapping based on ``$XONSH_COLOR_STYLE``.  The associated ``xonsh_style_proxy()``
  is intended for wrapping ``XonshStyle`` when actually being used by
  pygments.
* The functions ``print_color()`` and ``format_color()`` found in ``xonsh.tools``
  dispatch to the approriate shell color handling and may be used from
  anywhere.
* ``xonsh.tools.HAVE_PYGMENTS`` flag now denotes if pygments is installed and
  available on the users system.
* The ``ansi_colors`` module is now availble for handling ANSI color codes.
* ``?`` and ``??`` operator output now has colored titles, like in IPython.
* ``??`` will syntax highlight source code if pygments is available.
* Python mode output is now syntax highlighted if pygments is available.
* New ``$RIGHT_PROMPT`` environment variable for displaying right-aligned
  text in prompt-toolkit shell.
* Added ``!(...)`` operator, which returns an object representing the result
  of running a command.  The truth value of this object is True if the
  return code is equal to zero and False otherwise.
* Optional dependency on the win_unicode_console package to enable unicode
  support in cmd.exe on Windows. This can be disabled/enabled with the
  ``$WIN_UNICODE_CONSOLE`` environment variable.

**Changed:**

* Updated ``$SHELL_TYPE`` default to ``'best'``.
* Shell classes are now responsible for implementing their own color
  formatting and printing.
* Prompt coloring, history diffing, and tracing uses new color handling
  capabilities.
* New ``Token.Color`` token for xonsh color names, e.g. we now use
  ``Token.Color.RED`` rather than ``Token.RED``.
* Untracked files in git are ignored when determining if a git workdir is
  is dirty. This affects the coloring of the branch label.
* Regular expression globbing now uses ``re.fullmatch`` instead of
  ``re.match``, and the result of an empty regex glob does not cause the
  argument to be deleted.


**Removed:**

* The ``xonsh.tools.TERM_COLORS`` mapping has been axed, along with all
  references to it. This may cause a problem if you were using a raw color code
  in your xonshrc file from ``$FORMATTER_DICT``. To fix, simply remove these
  references.

**Fixed:**

* Multidimensional slicing, as in numpy, no longer throws SyntaxErrors.
* Some minor zsh fixes for more platforms and setups.
* The ``BaseShell.settitle`` method no longer has its commands captured by
  ``$(...)``



v0.2.6
====================
**Added:**

* ``trace`` alias added that enables users to turn on and off the printing
  of source code lines prior to their execution. This is useful for debugging scripts.
* New ability to force callable alias functions to be run in the foreground, i.e.
  the main thread from which the function was called. This is useful for debuggers
  and profilers which may require such access. Use the ``xonsh.proc.foreground``
  decorator on an alias function to flag it. ``ForegroundProcProxy`` and
  ``SimpleForegroundProcProxy`` classes have been added to support this feature.
  Normally, forcing a foreground alias is not needed.
* Added boolean ``$RAISE_SUBPROC_ERROR`` environment variable. If true
  and a subprocess command exits with a non-zero return code, a
  CalledProcessError will be raised. This is useful in scripts that should
  fail at the first error.
* If the ``setproctitle`` package is installed, the process title will be
  set to ``'xonsh'`` rather than the path to the Python interpreter.
* zsh foreign shell interface now supported natively in xonsh, like with Bash.
  New ``source-zsh`` alias allows easy access to zsh scripts and functions.
* Vox virtual environment manager added.

**Changed:**

* The ``foreign_shell_data()`` keyword arguments ``envcmd`` and ``aliascmd``
  now default to ``None``.
* Updated alias docs to pull in usage from the commands automatically.

**Fixed:**

* Hundreds of bugs related to line and column numbers have been addressed.
* Fixed path completion not working for absolute paths or for expanded paths on Windows.
* Fixed issue with hg dirty branches and $PATH.
* Fixed issues related to foreign shell data in files with whitespace in the names.
* Worked around bug in ConEmu/cmder which prevented ``get_git_branch()``
  from working in these terminal emulators on Windows.


v0.2.5
===========
**Added:**

* New configuration utility 'xonfig' which reports current system
  setup information and creates config files through an interactive
  wizard.
* Toolkit for creating wizards now available
* timeit and which aliases will now complete their arguments.
* $COMPLETIONS_MENU_ROWS environment variable controls the size of the
  tab-completion menu in prompt-toolkit.
* Prompt-toolkit shell now supports true multiline input with the ability
  to scroll up and down in the prompt.

**Changed:**

* The xonfig wizard will run on interactive startup if no configuration
  file is found.
* BaseShell now has a singleline() method for prompting a single input.
* Environment variable docs are now auto-generated.
* Prompt-toolkit shell will now dynamically allocate space for the
  tab-completion menu.
* Looking up nonexistent environment variables now generates an error
  in Python mode, but produces a sane default value in subprocess mode.
* Environments are now considered to contain all manually-adjusted keys,
  and also all keys with an associated default value.

**Removed:**

* Removed ``xonsh.ptk.shortcuts.Prompter.create_prompt_layout()`` and
  ``xonsh.ptk.shortcuts.Prompter.create_prompt_application()`` methods
  to reduce portion of xonsh that forks prompt-toolkit. This may require
  users to upgrade to prompt-toolkit v0.57+.

**Fixed:**

* First prompt in the prompt-toolkit shell now allows for up and down
  arrows to search through history.
* Made obtaining the prompt-toolkit buffer thread-safe.
* Now always set non-detypable environment variables when sourcing
  foreign shells.
* Fixed issue with job management if a TTY existed but was not controlled
  by the process, posix only.
* Jupyter kernel no longer times out when using foreign shells on startup.
* Capturing redirections, e.g. ``$(echo hello > f.txt)``, no longer fails
  with a decoding error.
* Evaluation in a Jupyter cell will return pformatted object.
* Jupyter with redirect uncaptured subprocs to notebook.
* Tab completion in Jupyter fixed.


v0.2.1 - v0.2.4
===============
You are reading the docs...but you still feel hungry.

v0.2.0
=============
**Added:**

* Rich history recording and replaying

v0.1.0
=============
**Added:**

* Naturally typed environment variables
* Inherits the environment from BASH
* Uses BASH completion for subprocess commands
* Regular expression filename globbing
* Its own PLY-based lexer and parser
* xonsh code parses into a Python AST
* You can do all the normal Python things, like arithmetic and importing
* Captured and uncaptured subprocesses
* Pipes, redirection, and non-blocking subprocess syntax support
* Help and superhelp with ? and ??
* Command aliasing
* Multiline input, unlike ed
* History matching like in IPython
* Color prompts
* Low system overhead




<v0.1.0
=============
The before times, like 65,000,000 BCE.
