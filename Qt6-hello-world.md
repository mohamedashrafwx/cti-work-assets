
# Table of Contents

1.  [Qt-6 Hello world](#orgdd468fa)
    1.  [EMACS](#org17b7c1d)
2.  [Qt-6 Hello world (continued)](#org070550e)
3.  [Qt-6 Hello world (cpp way)](#org8c8ae65)
4.  [Qt-6 Hello world (qt way)](#org3577c3d)
5.  [Qt-6 Hello world (QDebug)](#org44e0e73)


<a id="orgdd468fa"></a>

# Qt-6 Hello world

Open qt-6 creator and create a new project. Choose console application. Choose cmake build system.


<a id="org17b7c1d"></a>

## EMACS

for emacs, when you open the project, lsp won't recognize Qt shit. you need to export compile commands as follows:

-   go to your cmake build directory:

    cmake .. -DCMAKE_EXPORT_COMPILE_COMMANDS=ON

This Configures Qt correctly, and most importantly, generates "build/compile<sub>commands.json</sub>"

Then when you want to build:

    cmake --build .

this builds your project.


<a id="org070550e"></a>

# Qt-6 Hello world (continued)

once you have your project set up, you will find something like this:

    #include <QCoreApplication>
    
    int main(int argc, char *argv[])
    {
        QCoreApplication a(argc, argv);
    
        // do you things here
    
        return a.exec();
    }

The first line in the main creates an event loop, and you enter that event loop in the return expression.
if you build this, you get a console app that keeps running without doing anything. This is actually the event loop runnging.


<a id="org8c8ae65"></a>

# Qt-6 Hello world (cpp way)

Now let's add some action.

    #include <QCoreApplication>
    #include <iostream>
    
    void do_cpp()
    {
            std::string name;
            std::cout << "Please enter your name: ";
            std::cin >> name;
            std::cout << "Hello " << name << std::endl;
    }
    
    int main(int argc, char *argv[])
    {
        QCoreApplication a(argc, argv);
    
        do_cpp();
    
        return a.exec();
    }

here we created a normal cpp function called do<sub>cpp</sub>, and called it in the main after creating the event loop and before entering it.
the app does exactly what you expect, it asks for your name and takes it from you and reply with a hello to you.
Then it enters the event loop and keeps running&#x2026;


<a id="org3577c3d"></a>

# Qt-6 Hello world (qt way)

Now let's try use some Qt handy stuff instead of raw cpp.

    #include <QCoreApplication>
    #include <QTextStream>
    
    void do_qt()
    {
            QTextStream qin(stdin);
            QTextStream qout(stdout);
    
            qout << "Please enter your name:" << Qt::flush;
            qout.flush();
            QString name = qin.readLine();
            qout << "Hello"<< name << Qt::flush;
            qout.flush();
    }
    
    
    int main(int argc, char *argv[])
    {
        QCoreApplication a(argc, argv);
        do_qt();
        return a.exec();
    }

Notice that we didn't even include <iostream>, we included something else called <QTextStream>.
If you take this name and go to the manual ref, it will tell you this:
"The QTextStream class provides a convenient interface for reading and writing text."

First you consruct a QTextStream, and give it the file(device) or whatever, there are many overloads..

Then you can write out to the stdout (you need to add Qt::flush for emediate output) as you use std::cout&#x2026;
Then you can read a line with the input stream with readLine() method, it returens a QString not an std::string.


<a id="org44e0e73"></a>

# Qt-6 Hello world (QDebug)

Another important class is QDebug.

    #include <QCoreApplication>
    #include <QTextStream>
    #include <QDebug>
    
    void do_mixed()
    {
            QTextStream qin(stdin);
            qInfo() << "Please enter your name:";
            QString name = qin.readLine();
            qInfo() << "Hello"<< name;
    }
    
    
    int main(int argc, char *argv[])
    {
        QCoreApplication a(argc, argv);
        do_mixed();
        return a.exec();
    }

The QDebug is actually included in the <QCoreApplication> class, but we include it here to be explicit.
This gives you a rich logging system. You can do qInfo(), qDebug(), qWarning(), etc,..

