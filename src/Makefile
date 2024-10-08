# This file is a part of trayIt utility.
# Copyright © 2016-2019 Видершпан Евгений Сергеевич
#
# Permission is hereby granted, free of charge, to any person obtaining
# a copy of this software and associated documentation files (the
# "Software"), to deal in the Software without restriction, including
# without limitation the rights to use, copy, modify, merge, publish,
# distribute, sublicense, and/or sell copies of the Software, and to
# permit persons to whom the Software is furnished to do so, subject to
# the following conditions:
#
# The above copyright notice and this permission notice shall be included
# in all copies or substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
# EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
# MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
# IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
# CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
# TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
# SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

# Built with PSDK minimum version 5.0 or later (win98+)
!IFNDEF  APPVER
!MESSAGE nmake: APPVER not defined, set APPVER to 5.0 (default)
APPVER   = 5.0
OPTS     = APPVER="5.0"
!ENDIF

!include <win32.mak>

!IF $(NMAKE_WINVER) == 0x0400
!ERROR   nmake: minimum PSDK version is 5.0 (use: nmake APPVER="5.0")
!ENDIF

# old PSDKs set the /PDB:NONE option by default, REMOVE it
!IFNDEF NODEBUG
lflags = $(lflags:/PDB:NONE=)
!ENDIF

!IFNDEF  OUTDIR
!IFNDEF  NODEBUG
OUTDIR   = BUILD\PSDK$(APPVER)\Debug
!ELSE
OUTDIR   = BUILD\PSDK$(APPVER)\Release
!ENDIF
!ENDIF


PROJECT  = trayit

# sources
PROJ_SRC = \
            trayit.c

# compilation objects
PROJ_OBJS   = \
     $(OUTDIR)\trayit.obj

# libraries
EXT_LIBS    = $(olelibsmt) shell32.lib comctl32.Lib

# resources
RC_DEP      = icon.ico
RC_FILE     = rsrc.rc

# compilation flags
BUILD_FLAGS = $(cdebug) $(cflags) $(cvarsmt)

# file with flags of the previous build
BUILD_INFO  = $(OUTDIR)\build.info


all: $(OUTDIR) .flags $(OUTDIR)\$(PROJECT).exe

$(OUTDIR):
     if not exist "$(OUTDIR)/$(NULL)" mkdir "$(OUTDIR)"

# update build flags file (if they change)
.flags:
    @ <<~flags.bat
    @SET CURRENT_FLAGS=
    @IF EXIST $(BUILD_INFO) SET /p CURRENT_FLAGS=< $(BUILD_INFO)
    @IF "%CURRENT_FLAGS%" NEQ "$(cc) $(BUILD_FLAGS)" <NUL SET /p="$(cc) $(BUILD_FLAGS)" > $(BUILD_INFO)
    @EXIT 0
<<

# when changing flags - complete rebuild!
$(PROJ_SRC): $(BUILD_INFO)

$(OUTDIR)\$(PROJECT).res: $(RC_FILE) $(RC_DEP) $(BUILD_INFO)
    $(rc) $(rcflags) $(rcvars) /fo $@ $(RC_FILE)

.c{$(OUTDIR)\}.obj:
    $(cc) $(BUILD_FLAGS) /Fo"$(OUTDIR)\\" /Fd"$(OUTDIR)\\" $**

.cpp{$(OUTDIR)\}.obj:
    $(cc) $(BUILD_FLAGS) /Fo"$(OUTDIR)\\" /Fd"$(OUTDIR)\\" $**

$(OUTDIR)\$(PROJECT).exe: $(PROJ_OBJS) $(OUTDIR)\$(PROJECT).res
    $(link) $(linkdebug) $(guilflags) $** $(EXT_LIBS) -out:$@ $(MAPFILE)

clean:
     $(CLEANUP)

