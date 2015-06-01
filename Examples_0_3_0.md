# java.lang.Runtime #
Running the converter on [java/lang/Runtime.java](http://hg.openjdk.java.net/jdk6/jdk6/jdk/file/99b43838c5d0/src/share/classes/java/lang/Runtime.java) and its dependencies produces ~2000 classes (direct and indirect dependencies).

For Runtime itself, four files are generated. Had Runtime had any nested classes, separate files would have been generated for each of those.

First is the forward declaration header which contains forward declarations of the classes involved. Currently, a forward header is generated for each package containing all classes of that package.

`java.lang.Runtime.h` contains the class declaration, `java.lang.Runtime.cpp` definitions and `java.lang.Runtime-native.cpp` the blanks you'd have to fill in to make Runtime work.

Generated using version 0.3.0 of J2C.

## java/lang/Runtime.hpp ##
```
// Generated from /openjdk6/java/lang/Runtime.java

#pragma once

#include <fwd.hpp>
#include <java/io/fwd.hpp>
#include <java/lang/Object.hpp>

struct default_init_tag;

class java::lang::Runtime
    : public virtual Object
{

public:
    typedef Object super;

private:
    static Runtime *currentRuntime_;

public:
    static Runtime *getRuntime();
protected:
    void ctor();

public:
    virtual void exit(int32_t status);
    virtual void addShutdownHook(Thread *hook);
    virtual bool removeShutdownHook(Thread *hook);
    virtual void halt(int32_t status);
    static void runFinalizersOnExit(bool value);
    virtual Process *exec(String *command) /* throws(IOException) */;
    virtual Process *exec(String *command, StringArray *envp) /* throws(IOException) */;
    virtual Process *exec(String *command, StringArray *envp, ::java::io::File *dir) /* throws(IOException) */;
    virtual Process *exec(StringArray *cmdarray) /* throws(IOException) */;
    virtual Process *exec(StringArray *cmdarray, StringArray *envp) /* throws(IOException) */;
    virtual Process *exec(StringArray *cmdarray, StringArray *envp, ::java::io::File *dir) /* throws(IOException) */;
    virtual int32_t availableProcessors();
    virtual int64_t freeMemory();
    virtual int64_t totalMemory();
    virtual int64_t maxMemory();
    virtual void gc();

private:
    static void runFinalization0();

public:
    virtual void runFinalization();
    virtual void traceInstructions(bool on);
    virtual void traceMethodCalls(bool on);
    virtual void load(String *filename);

public: /* package */
    virtual void load0(Class *fromClass, String *filename);

public:
    virtual void loadLibrary(String *libname);

public: /* package */
    virtual void loadLibrary0(Class *fromClass, String *libname);

public:
    virtual ::java::io::InputStream *getLocalizedInputStream(::java::io::InputStream *in);
    virtual ::java::io::OutputStream *getLocalizedOutputStream(::java::io::OutputStream *out);

    // Generated
protected:
    Runtime();
    Runtime(const ::default_init_tag&);


public:
    static ::java::lang::Class *class_();
    static void clinit();

private:
    static Runtime *&currentRuntime();
    virtual ::java::lang::Class* getClass0();
};
```

## java.lang.Runtime.cpp ##
```
// Generated from /openjdk6/java/lang/Runtime.java
#include <java/lang/Runtime.hpp>

#include <java/io/File.hpp>
#include <java/io/InputStream.hpp>
#include <java/io/OutputStream.hpp>
#include <java/lang/ApplicationShutdownHooks.hpp>
#include <java/lang/ClassLoader.hpp>
#include <java/lang/IllegalArgumentException.hpp>
#include <java/lang/NullPointerException.hpp>
#include <java/lang/Process.hpp>
#include <java/lang/ProcessBuilder.hpp>
#include <java/lang/RuntimePermission.hpp>
#include <java/lang/SecurityException.hpp>
#include <java/lang/SecurityManager.hpp>
#include <java/lang/Shutdown.hpp>
#include <java/lang/String.hpp>
#include <java/lang/StringBuilder.hpp>
#include <java/lang/System.hpp>
#include <java/lang/UnsatisfiedLinkError.hpp>
#include <java/util/StringTokenizer.hpp>
#include <java/lang/StringArray.hpp>

template<typename T>
static T* npc(T* t)
{
    if(!t) throw new ::java::lang::NullPointerException();
    return t;
}

::java::lang::Runtime::Runtime(const ::default_init_tag&)
{
    clinit();
}

::java::lang::Runtime::Runtime() 
    : Runtime(*static_cast< ::default_init_tag* >(0))
{
    ctor();
}

::java::lang::Runtime *&::java::lang::Runtime::currentRuntime()
{
    clinit();
    return currentRuntime_;
}
::java::lang::Runtime *::java::lang::Runtime::currentRuntime_;

::java::lang::Runtime *::java::lang::Runtime::getRuntime()
{
    clinit();
    return currentRuntime_;
}

void ::java::lang::Runtime::ctor()
{
    super::ctor();
}

void ::java::lang::Runtime::exit(int32_t status)
{
    SecurityManager *security = System::getSecurityManager();
    if(security != nullptr) {
        npc(security)->checkExit(status);
    }
    Shutdown::exit(status);
}

void ::java::lang::Runtime::addShutdownHook(Thread *hook)
{
    SecurityManager *sm = System::getSecurityManager();
    if(sm != nullptr) {
        npc(sm)->checkPermission((new RuntimePermission(u"shutdownHooks"_j)));
    }
    ApplicationShutdownHooks::add(hook);
}

bool ::java::lang::Runtime::removeShutdownHook(Thread *hook)
{
    SecurityManager *sm = System::getSecurityManager();
    if(sm != nullptr) {
        npc(sm)->checkPermission((new RuntimePermission(u"shutdownHooks"_j)));
    }
    return ApplicationShutdownHooks::remove(hook);
}

void ::java::lang::Runtime::halt(int32_t status)
{
    SecurityManager *sm = System::getSecurityManager();
    if(sm != nullptr) {
        npc(sm)->checkExit(status);
    }
    Shutdown::halt(status);
}

void ::java::lang::Runtime::runFinalizersOnExit(bool value)
{
    clinit();
    SecurityManager *security = System::getSecurityManager();
    if(security != nullptr) {
        try {
            npc(security)->checkExit(int32_t(0));
        } catch (SecurityException *e) {
            throw (new SecurityException(u"runFinalizersOnExit"_j));
        }
    }
    Shutdown::setRunFinalizersOnExit(value);
}

::java::lang::Process *::java::lang::Runtime::exec(String *command) /* throws(IOException) */
{
    return exec(command, static_cast< StringArray* >(nullptr), static_cast< ::java::io::File* >(nullptr));
}

::java::lang::Process *::java::lang::Runtime::exec(String *command, StringArray *envp) /* throws(IOException) */
{
    return exec(command, envp, static_cast< ::java::io::File* >(nullptr));
}

::java::lang::Process *::java::lang::Runtime::exec(String *command, StringArray *envp, ::java::io::File *dir) /* throws(IOException) */
{
    if(npc(command)->length() == int32_t(0))
        throw (new IllegalArgumentException(u"Empty command"_j));

    ::java::util::StringTokenizer *st = (new ::java::util::StringTokenizer(command));
    StringArray *cmdarray = (new StringArray(npc(st)->countTokens()));
    for (int32_t  i = int32_t(0); npc(st)->hasMoreTokens(); i++) 
                cmdarray->set(i, npc(st)->nextToken());

    return exec(cmdarray, envp, dir);
}

::java::lang::Process *::java::lang::Runtime::exec(StringArray *cmdarray) /* throws(IOException) */
{
    return exec(cmdarray, static_cast< StringArray* >(nullptr), static_cast< ::java::io::File* >(nullptr));
}

::java::lang::Process *::java::lang::Runtime::exec(StringArray *cmdarray, StringArray *envp) /* throws(IOException) */
{
    return exec(cmdarray, envp, static_cast< ::java::io::File* >(nullptr));
}

::java::lang::Process *::java::lang::Runtime::exec(StringArray *cmdarray, StringArray *envp, ::java::io::File *dir) /* throws(IOException) */
{
    return npc(npc((new ProcessBuilder(cmdarray))->environment(envp))->directory(dir))->start();
}

void ::java::lang::Runtime::runFinalization()
{
    runFinalization0();
}

void ::java::lang::Runtime::load(String *filename)
{
    load0(System::getCallerClass(), filename);
}

void ::java::lang::Runtime::load0(Class *fromClass, String *filename)
{
    SecurityManager *security = System::getSecurityManager();
    if(security != nullptr) {
        npc(security)->checkLink(filename);
    }
    if(!((new ::java::io::File(filename))->isAbsolute())) {
        throw (new UnsatisfiedLinkError(::java::lang::StringBuilder().append(u"Expecting an absolute path of the library: "_j)->append(filename)->toString()));
    }
    ClassLoader::loadLibrary(fromClass, filename, true);
}

void ::java::lang::Runtime::loadLibrary(String *libname)
{
    loadLibrary0(System::getCallerClass(), libname);
}

void ::java::lang::Runtime::loadLibrary0(Class *fromClass, String *libname)
{
    SecurityManager *security = System::getSecurityManager();
    if(security != nullptr) {
        npc(security)->checkLink(libname);
    }
    if(npc(libname)->indexOf(static_cast< int32_t >(::java::io::File::separatorChar())) != -int32_t(1)) {
        throw (new UnsatisfiedLinkError(::java::lang::StringBuilder().append(u"Directory separator should not appear in library name: "_j)->append(libname)->toString()));
    }
    ClassLoader::loadLibrary(fromClass, libname, false);
}

::java::io::InputStream *::java::lang::Runtime::getLocalizedInputStream(::java::io::InputStream *in)
{
    return in;
}

::java::io::OutputStream *::java::lang::Runtime::getLocalizedOutputStream(::java::io::OutputStream *out)
{
    return out;
}

extern ::java::lang::Class *class_(const char16_t *c, int n);

::java::lang::Class *::java::lang::Runtime::class_()
{
    static ::java::lang::Class *c = ::class_(u"java.lang.Runtime", 17);
    return c;
}

void ::java::lang::Runtime::clinit()
{
    super::clinit();
    static bool in_cl_init = false;
struct clinit_ {
    clinit_() {
        in_cl_init = true;
    currentRuntime_ = (new Runtime());
    }
};

    if(!in_cl_init) {
        static clinit_ clinit_instance;
    }
}

::java::lang::Class *::java::lang::Runtime::getClass0()
{
    return class_();
}
```

## java/lang/Runtime-native.cpp ##
```
// Generated from /openjdk6/java/lang/Runtime.java
#include <java/lang/Runtime.hpp>


int32_t ::java::lang::Runtime::availableProcessors()
{ /* native */
    return 0;
}

int64_t ::java::lang::Runtime::freeMemory()
{ /* native */
    return 0;
}

void ::java::lang::Runtime::gc()
{ /* native */
}

int64_t ::java::lang::Runtime::maxMemory()
{ /* native */
    return 0;
}

void ::java::lang::Runtime::runFinalization0()
{ /* native */
    clinit();
}

int64_t ::java::lang::Runtime::totalMemory()
{ /* native */
    return 0;
}

void ::java::lang::Runtime::traceInstructions(bool on)
{ /* native */
}

void ::java::lang::Runtime::traceMethodCalls(bool on)
{ /* native */
}
```
