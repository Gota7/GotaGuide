# How To Make A Programming Language
Ever wanted to create your own programming language?

## Table Of Contents
* [Index](Index.md)
* [ANTLR4 Setup](AntlrSetup.md) <- You are here.
* [ANTLR4 Grammar](Grammar.md)
* [Creating Toylet](CreatingToylet.md)
* [Project Setup](ProjectSetup.md)
* [LLVM Crashcourse](Llvm.md)
* [Compiling Toylet](CompilingToylet.md)
* [Epilogue](Epilogue.md)

## Basic Setup
Please don't use the default packages from package managers! First thing you need to do is download [here](https://www.antlr.org/download.html). You want to download the jar linked under "ANTLR tool and Java Target". You also need to have Java installed. I recommend copying it to a safe location, which I recommend `/usr/local/lib` for Linux. You run this command as `java -jar JAR_FILE_PATH`, which I will shorthand as `antlr4` for the rest of the tutorial. To get this shortcut to be `antlr4` on linux, edit `~/.bashrc`, put in the line `alias antlr4='java -jar /usr/local/lib/antlr-4.9.2-complete.jar'` where the path is to your jar file, which may be different from mine. Do `source ~/.bashrc` to refresh, and `antlr4` should be a recognized command! For Windows, you have to append the path to the jar file to the `CLASSPATH` environment variable, or create it if it does not exist.

## Testing Environment Setup
You need the classpath environment variable to be appended with a value. For Linux, edit your `~/.bashrc` to have this line: `export CLASSPATH=".:/usr/local/lib/antlr-4.9.2-complete.jar:$CLASSPATH"` where the path to the jar is where yours is. Also put in this line `alias grun='java org.antlr.v4.gui.TestRig'` so that way we can test data easily. `java org.antlr.v4.gui.TestRig` is how we will test our grammar, and I will refer to this command as `grun` from now on. If you are on Windows, you probably have to edit your environment variables manually for the CLASSPATH variable. You can use `doskey` instead of alias. You can also use the full command abbreviated by them. Do `source ~/.bashrc` to refresh.

## Does It Work?
If you can run `antlr4` or `java -jar /usr/local/lib/antlr-4.9.2-complete.jar` where the jar is the path to your jar file, you got ANTLR set up! If you can do `grun` or `java org.antlr.v4.gui.TestRig`, you can test your grammars too!

## Next
Next thing to do is learn ANTLR4 syntax!
[ANTLR4 Grammar](Grammar.md)