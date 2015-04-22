# About #

Goconf is a configuration file parser for the [Go Programming Language](http://golang.org). It is based on [goconfig](http://github.com/msbronco/goconfig).

Goconf has a few new features:

  * It gives more detailed errors
  * There is no need to specify a section name in the configuration file ("default" is assumed as the section name)
  * It can now read from a byte slice or io.Reader
  * It can now write to a byte slice or io.Writer
  * **It Compiles!** It works with go version 1 and later.

# Installation #

You can just link your program to goconf, but it is suggested that you install this library in $GOROOT with goinstall.

## Goinstall ##

Goinstall will automatically fetch the latest code and it will even update it for you if you tell it to. As soon as the library stabilizes it will only fetch the latest stable release.

```
goinstall goconf.googlecode.com/hg
```

Then, when you want to use it you just need to `import "goconf.googlecode.com/hg"`

# Using It #

You can use goconf by putting the correct import statement (depending on which of the above methods you used) at the top of the file.

**NOTE:** All section names and options are case insensitive. All values are case sensitive.

## Example 1 ##

### Config ###
```
host = something.com
port = 443
active = true
compression = off
```

### Code ###

```
c, err := conf.ReadConfigFile("something.config")
c.GetString("default", "host") // return something.com
c.GetInt("default", "port") // return 443
c.GetBool("default", "active") // return true
c.GetBool("default", "compression") // return false
```

## Example 2 ##

### Config ###

```
[default]
host = something.com
port = 443
active = true
compression = off

[service-1]
compression = on

[service-2]
port = 444
```

### Code ###

```
c, err := conf.ReadConfigFile("something.config")
c.GetBool("default", "compression") // returns false
c.GetBool("service-1", "compression") // returns true
c.GetBool("service-2", "compression") // returns GetError
```

# More Documentation #
You can get more documentation by using godoc. Just run `godoc -http=:6060` (assuming you used goinstall) and then in a web browser go to http://localhost:6060/pkg/gconf.goooglecode.com/hg. There you will find a list off all the functions and descriptions for each. These descriptions need work so I am willing to take any patches or ideas to fix it up.

# TODO #
You can tell a lot about the state of a library by this list of "to dos".

  * make "global" a synonym for "default" (I like global better. It is purely cosmetic)
  * when printing configuration files make the default section come first
  * create better documentation
    * Add information on generating configuration files
  * Recreate the string substitution feature.