# Java

I prefer to manage my Java installs with IntelliJ. It puts them in `~/.jdks/`.

To make sure I can use an IntelliJ installed Java from from the command line (zsh) I run the following command:

    J=/home/$USER/.jdks/corretto-18.0.2 printf "\nexport JAVA_HOME=$J\nexport JAVA_ROOT=$J\nexport JAVA_BINDIR=$J/bin\n" >> ~/.zshrc
    
This assumes the installed Java is `corretto-18.0.2` but that could be different.
