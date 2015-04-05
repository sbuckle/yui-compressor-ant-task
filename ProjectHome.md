# Introduction #

This project provides a custom Ant task for compressing JavaScript and CSS files using [YUI Compressor](http://developer.yahoo.com/yui/compressor/).

More information here: http://www.simonbuckle.com/2011/02/20/yui-compressor-ant-task/

# Ant Task #

The name of the Ant task is "yuicompressor" (see the example below). It supports the following attributes:

| **Attribute** | **Required** | **Default Value** |
|:--------------|:-------------|:------------------|
| linebreak | No | -1 |
| munge | No | true |
| preserveAllSemiColons | No | false |
| disableOptimizations | No | false |
| verbose | No | false |
| todir | Yes | _N/A_ |

A full description of the attributes (apart from todir) can be found on the YUI Compressor site: http://developer.yahoo.com/yui/compressor/

It also expects two child elements: fileset and mapper. **These are required elements**.
The first specifies the list of files to compress; the second prescribes how they should be saved once minimised.

To build it run: `ant package`.

# Example #

Here's an example build file that looks for all the JavaScript files in the test directory and minimises them:
```
<project name="Compressor Test" default="example" basedir=".">
        
        <taskdef resource="yuicompressor.tasks" classpath="dist/yuicompressor-taskdef-1.0.jar"/>
        
        <target name="example">
                <yuicompressor linebreak="40" todir="/var/www">
                        <fileset dir="${basedir}/test" includes="*.js"/>
                        <mapper type="glob" from="*.js" to="*-min.js"/>
                </yuicompressor>
        </target>

</project>
```
Save the file as `build.example.xml` in the root directory of the project. To run it, type the following: `ant -lib lib/yuicompressor-2.4.2.jar -f build.example.xml`.

(The task relies on YUI Compressor so this must be specified using the -lib option.)

Here's another example that compresses both CSS and JavaScript files:
```
<project name="Compressor Test" default="example" basedir=".">
        
        <taskdef resource="yuicompressor.tasks" classpath="dist/yuicompressor-taskdef-1.0.jar"/>
        
        <target name="example">
                <yuicompressor linebreak="40" todir="/var/www">
                        <fileset dir="${basedir}/test" includes="*.css,*.js"/>
                        <mapper>
                              <globmapper from="*.css" to="*-min.css"/>
                              <globmapper from="*.js" to="*-min.js"/>
                        </mapper>
                </yuicompressor>
        </target>

</project>
```