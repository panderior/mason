### Distributed MASON Makefile
#### By Sean Luke

#### Relevant Stuff:
#### To see all your make options:  type   make help
#### To add flags (like -O) to javac:  change the FLAGS variable below

JAVAC = javac ${JAVACFLAGS}

CLASSROOT = target/classes
JAVACFLAGS = -target 1.8 -source 1.8 ${FLAGS} -nowarn -d ${CLASSROOT}
FLAGS = -g -Xlint:deprecation -cp src/main/java/:${CLASSPATH}

VERSION = 1

# Main java files, not including the 3D stuff
SRCS = \
sim/app/dantsforage/*.java \
sim/app/dflockers/*.java \
sim/app/dflockers/display/*.java \
sim/app/dheatbugs/*.java \
sim/app/dheatbugs/display/*.java \
sim/app/dschelling/*.java \
sim/engine/*.java \
sim/engine/mpi/*.java \
sim/engine/rmi/*.java \
sim/field/*.java \
sim/field/continuous/*.java \
sim/field/grid/*.java \
sim/field/partitioning/*.java \
sim/field/storage/*.java \
sim/util/*.java \
sim/display/*.java \
sim/app/dflockers/display/*.java \
sim/app/dheatbugs/display/*.java \
#sim/app/dwanderer/*.java \

RSRCROOT=src/main/resources/
SRCROOT=src/main/java/
DIRS=$(addprefix $(SRCROOT)/, $(SRCS))
3DDIRS=$(addprefix $(SRCROOT)/, $(3DSRCS))

# Make the main Distributed MASON code
all:
	@ echo This builds Distributed MASON
	@ echo To learn about other options, type 'make help'
	@ echo In order to build, first be certain that the MASON jar is in your CLASSPATH
	@ echo You also need to have installed OpenMPI and added the Open MPI jar file
	@ echo to your CLASSPATH
	@ echo Finally, be sure that the Fast Serialization library from 
	@ echo distributed-libraries.zip is also in your CLASSPATH.
	@ echo Read Section 14.3 of the MASON Manual for more detailed instructions
	@ echo
	mkdir -p ${CLASSROOT}
	${JAVAC} ${DIRS}

# Make the main MASON code AND the 3D code
visualization: all
	mkdir -p ${CLASSROOT}
	${JAVAC} ${DIRS} ${3DDIRS}
	tar -C ${RSRCROOT} -c sim ec | tar -C ${CLASSROOT} -x # Hack to combine resources with class files


# Delete all jmf gunk, checkpoints, backup emacs gunk classfiles,
# documentation, and odd MacOS X poops
clean:
	find . -name "*.class" -exec rm -f {} \;
	find . -name "jmf.log" -exec rm -f {} \;
	find . -name ".DS_Store" -exec rm -f {} \; 
	find . -name "*.checkpoint" -exec rm -f {} \;
	find . -name "*.java*~" -exec rm -f {} \;
	find . -name ".#*" -exec rm -rf {} \;
	rm -rf target/*.jar docs/classdocs/resources docs/classdocs/ec docs/classdocs/sim docs/classdocs/*.html docs/classdocs/*.css docs/classdocs/package*
	rm -rf ${CLASSROOT}
	rm -rf target/


# Build a jar file.
jar: all
	touch /tmp/manifest.add
	rm /tmp/manifest.add
	echo "Main-Class: sim.display.Console" > /tmp/manifest.add
	jar -cvfm target/distributed-mason.${VERSION}.jar /tmp/manifest.add -C ${CLASSROOT} .


# Build the class docs.  They're located in docs/classdocs
# the "-" at the beginning causes Make to ignore errors from javadoc
doc:
	- javadoc -classpath ${CLASSPATH}:${SRCROOT} -protected -d docs/classdocs sim.display sim.engine sim.util sim.util.gui sim.util.media sim.util.media.chart sim.field sim.field.grid sim.field.continuous sim.field.network sim.portrayal sim.portrayal.grid sim.portrayal.continuous sim.portrayal.network sim.portrayal.simple ec.util sim.portrayal3d sim.portrayal3d.grid sim.portrayal3d.continuous sim.portrayal3d.simple sim.portrayal3d.grid.quad sim.display3d sim.util.distribution sim.engine.registry sim.engine.transport sim.field.partitioning sim.field.proxy sim.field.storage 

jar: all
	touch /tmp/manifest.add
	rm /tmp/manifest.add
	echo "Main-Class: sim.display.Console" > /tmp/manifest.add
	jar -cvfm target/distributed-mason.${VERSION}.jar /tmp/manifest.add -C ${CLASSROOT} .

docs: doc

# Indent to your preferred brace format using emacs.  MASON's default
# format is Whitesmiths at 4 spaces.  Yes, I know.  Idiosyncratic.
# Anyway, beware that this is quite slow.  But it works!
indent: 
	touch ${HOME}/.emacs
	find ${SRCROOT} -name "*.java" -print -exec emacs --batch --load ~/.emacs --eval='(progn (find-file "{}") (mark-whole-buffer) (setq indent-tabs-mode nil) (untabify (point-min) (point-max)) (indent-region (point-min) (point-max) nil) (save-buffer))' \;

# Print a help message
help:
	@ echo Distributed MASON Makefile options
	@ echo
	@ echo "make          Builds Distributed MASON"
	@ echo "make all      (Same thing)"
	@ echo "make clean    Cleans out all classfiles, checkpoints, and various gunk"
	@ echo "make docs     Builds the class documentation, found in docs/classsdocs"
	@ echo "make doc      (Same thing)"
	@ echo "make jar      Builds, then collects ALL class files into a jar file"
	@ echo "              called" target/distributed-mason.${VERSION}.jar
	@ echo "make help     Brings up this message!"
	@ echo "make indent   Uses emacs to re-indent MASON java files as you'd prefer"
	@ echo


