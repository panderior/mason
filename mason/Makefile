### MASON Makefile
#### By Sean Luke

#### Relevant Stuff:
#### To see all your make options:  type   make help
#### To add flags (like -O) to javac:  change the FLAGS variable below

JAVAC = javac ${JAVACFLAGS}

CLASSROOT = target/classes
JAVACFLAGS = -target 1.8 -source 1.8 ${FLAGS} -nowarn -d ${CLASSROOT}
FLAGS = -g -Xlint:deprecation

#sim/app/serengeti/*.java \


VERSION = 22

# Main java files, not including the 3D stuff
SRCS = \
sim/app/heatbugs/*.java\
sim/app/hexabugs/*.java \
sim/app/asteroids/*.java \
sim/app/antsforage/*.java \
sim/app/virus/*.java \
sim/app/cto/*.java \
sim/app/woims/*.java \
sim/app/mav/*.java \
sim/app/mousetraps/*.java \
sim/app/networktest/*.java \
sim/app/keepaway/*.java \
sim/app/lsystem/*.java \
sim/app/lightcycles/*.java \
sim/app/swarmgame/*.java \
sim/app/flockers/*.java \
sim/app/tutorial1and2/*.java \
sim/app/tutorial3/*.java \
sim/app/tutorial4/*.java \
sim/app/tutorial5/*.java \
sim/app/schelling/*.java \
sim/app/pacman/*.java \
sim/app/pso/*.java \
sim/app/wcss/tutorial0*/*.java \
sim/app/wcss/tutorial10/*.java \
sim/app/wcss/tutorial11/*.java \
sim/app/wcss/tutorial12/*.java \
sim/app/wcss/tutorial13/*.java \
sim/display/*.java \
sim/engine/*.java \
sim/util/*.java \
sim/util/media/*.java \
sim/util/media/chart/*.java \
sim/util/gui/*.java \
sim/util/distribution/*.java \
sim/util/sweep/*.java \
sim/field/*.java \
sim/field/grid/*.java \
sim/field/continuous/*.java \
sim/field/network/*.java \
sim/portrayal/*.java \
sim/portrayal/grid/*.java \
sim/portrayal/continuous/*.java \
sim/portrayal/network/*.java \
sim/portrayal/simple/*.java \
sim/portrayal/inspector/*.java \
ec/util/*.java \


# The additional 3D java files
3DSRCS = \
sim/app/heatbugs3d/*.java \
sim/app/woims3d/*.java \
sim/app/mousetraps3d/*.java \
sim/app/crowd3d/*.java \
sim/app/balls3d/*.java \
sim/app/celegans/*.java \
sim/app/particles3d/*.java \
sim/app/pso3d/*.java \
sim/app/tutorial6/*.java \
sim/app/tutorial7/*.java \
sim/app/celegans/*.java \
sim/app/wcss/tutorial14/*.java \
sim/portrayal3d/*.java \
sim/portrayal3d/simple/*.java \
sim/portrayal3d/grid/*.java \
sim/portrayal3d/grid/quad/*.java \
sim/portrayal3d/continuous/*.java \
sim/portrayal3d/network/*.java \
sim/display3d/*.java \


RSRCROOT=src/main/resources/
SRCROOT=src/main/java/
DIRS=$(addprefix $(SRCROOT)/, $(SRCS))
3DDIRS=$(addprefix $(SRCROOT)/, $(3DSRCS))

# Make the main MASON code, not including 3D code
all:
	@ echo This makes the 2D MASON code.
	@ echo To learn about other options, type 'make help'
	@ echo
	mkdir -p ${CLASSROOT}
	${JAVAC} ${DIRS}
	tar -C ${RSRCROOT} -c sim ec | tar -C ${CLASSROOT} -x # Hack to combine resources with class files

# Make the main MASON code AND the 3D code
3d:
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

# Build the class docs.  They're located in docs/classdocs
# the "-" at the beginning causes Make to ignore errors from javadoc
doc:
	- javadoc -classpath ${CLASSPATH}:${SRCROOT} -protected -d docs/classdocs sim.display sim.engine sim.util sim.util.gui sim.util.media sim.util.media.chart sim.field sim.field.grid sim.field.continuous sim.field.network sim.portrayal sim.portrayal.grid sim.portrayal.continuous sim.portrayal.network sim.portrayal.simple ec.util sim.portrayal3d sim.portrayal3d.grid sim.portrayal3d.continuous sim.portrayal3d.simple sim.portrayal3d.grid.quad sim.display3d sim.util.distribution

docs: doc

# Build an applet jar file.  Note this collects ALL .class, .png, .jpg, index.html, and simulation.classes
# files.  you'll probably want to strip this down some.
jar: 3d
	touch /tmp/manifest.add
	rm /tmp/manifest.add
	echo "Main-Class: sim.display.Console" > /tmp/manifest.add
	jar -cvfm target/mason.${VERSION}.jar /tmp/manifest.add -C ${CLASSROOT} .

# Indent to your preferred brace format using emacs.  MASON's default
# format is Whitesmiths at 4 spaces.  Yes, I know.  Idiosyncratic.
# Anyway, beware that this is quite slow.  But it works!
indent: 
	touch ${HOME}/.emacs
	find ${SRCROOT} -name "*.java" -print -exec emacs --batch --load ~/.emacs --eval='(progn (find-file "{}") (mark-whole-buffer) (setq indent-tabs-mode nil) (untabify (point-min) (point-max)) (indent-region (point-min) (point-max) nil) (save-buffer))' \;


# Print a help message
help: 
	@ echo MASON Makefile options
	@ echo 
	@ echo "make          Builds the model core, utilities, and 2D code/apps only"
	@ echo "make all      (Same thing)"
	@ echo "make 3d       Builds the model core, utilities, and both 2D and 3D code/apps"
	@ echo "make docs     Builds the class documentation, found in docs/classsdocs"
	@ echo "make doc      (Same thing)"
	@ echo "make clean    Cleans out all classfiles, checkpoints, and various gunk"
	@ echo "make jar      Makes 3d, then collects ALL class files into a jar file"
	@ echo "              called" target/mason.${VERSION}.jar
	@ echo "make help     Brings up this message!"
	@ echo "make indent   Uses emacs to re-indent MASON java files as you'd prefer"
	@ echo

