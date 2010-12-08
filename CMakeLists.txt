# Copyright (C) 2007-2009 LuaDist.
# Submitted by David Manura
# Redistribution and use of this file is allowed according to the terms of the MIT license.
# For details see the COPYRIGHT file distributed with LuaDist.
# Please note that the package source code is licensed under its own license.

PROJECT (scintilla CXX)
CMAKE_MINIMUM_REQUIRED(VERSION 2.6)
INCLUDE (dist.cmake)

IF (WIN32)
  CONFIGURE_FILE(win32/ScintillaWin.cxx  ScintillaWinL.cxx  COPYONLY)
  CONFIGURE_FILE(win32/ScintillaWin.cxx  ScintillaWinS.cxx  COPYONLY)
  CONFIGURE_FILE(src/ScintillaBase.cxx   ScintillaBaseL.cxx COPYONLY)
ENDIF (WIN32)

INCLUDE_DIRECTORIES(src include)

# LDFLAGS=-mwindows -lstdc++ -limm32 -lole32 -luuid -mno-cygwin

# Add -MMD to get dependencies
##CXXFLAGS = -g -pg -pedantic -Os -fno-exceptions -fvtable-thunks -fno-rtti
#INCLUDEDIRS=-I ../include -I ../src
#CXXBASEFLAGS=-Wall -Wno-missing-braces -Wno-char-subscripts -pedantic $(INCLUDEDIRS) $(THUNKFLAGS) -fno-rtti -mno-cygwin

IF(MSVC)
  SET(DEF_FILE win32/Scintilla.def)
  SET(RC_FILE  win32/ScintRes.rc)
  ADD_DEFINITIONS(-D_CRT_SECURE_NO_WARNINGS)
ENDIF(MSVC)

# ScintillaWinS.o WindowAccessor.o

SET(LEXSRC
  src/LexAbaqus.cxx src/LexAda.cxx src/LexAPDL.cxx src/LexAsm.cxx
  src/LexAsn1.cxx src/LexASY.cxx src/LexAU3.cxx src/LexAVE.cxx src/LexBaan.cxx
  src/LexBash.cxx src/LexBasic.cxx src/LexBullant.cxx src/LexCaml.cxx
  src/LexCLW.cxx src/LexCmake.cxx src/LexCOBOL.cxx src/LexConf.cxx
  src/LexCPP.cxx src/LexCrontab.cxx src/LexCsound.cxx src/LexCSS.cxx
  src/LexD.cxx src/LexEiffel.cxx src/LexErlang.cxx src/LexEScript.cxx
  src/LexFlagship.cxx src/LexForth.cxx src/LexFortran.cxx src/LexGAP.cxx
  src/LexGui4Cli.cxx src/LexHaskell.cxx src/LexHTML.cxx src/LexInno.cxx
  src/LexKix.cxx src/LexLisp.cxx src/LexLout.cxx src/LexLua.cxx src/LexMagik.cxx
  src/LexMatlab.cxx src/LexMetapost.cxx src/LexMMIXAL.cxx src/LexMPT.cxx
  src/LexMSSQL.cxx src/LexMySQL.cxx src/LexNimrod.cxx src/LexNsis.cxx
  src/LexOpal.cxx src/LexOthers.cxx src/LexPascal.cxx src/LexPB.cxx
  src/LexPerl.cxx src/LexPLM.cxx src/LexPOV.cxx src/LexPowerPro.cxx
  src/LexPowerShell.cxx src/LexProgress.cxx src/LexPS.cxx src/LexPython.cxx
  src/LexR.cxx src/LexRebol.cxx src/LexRuby.cxx src/LexScriptol.cxx
  src/LexSmalltalk.cxx src/LexSML.cxx src/LexSorcus.cxx src/LexSpecman.cxx
  src/LexSpice.cxx src/LexSQL.cxx src/LexTACL.cxx src/LexTADS3.cxx
  src/LexTAL.cxx src/LexTCL.cxx src/LexTeX.cxx src/LexVB.cxx
  src/LexVerilog.cxx src/LexVHDL.cxx src/LexYAML.cxx )

SET(SSRC
  src/ScintillaBase.cxx src/Editor.cxx
  src/CharClassify.cxx src/Decoration.cxx src/Document.cxx
  src/ContractionState.cxx src/CellBuffer.cxx src/CallTip.cxx
  src/PositionCache.cxx src/KeyMap.cxx src/Indicator.cxx src/LineMarker.cxx
  src/RESearch.cxx src/RunStyles.cxx src/Selection.cxx src/Style.cxx
  src/ViewStyle.cxx src/AutoComplete.cxx src/UniConversion.cxx src/PropSet.cxx
  src/XPM.cxx src/PerLine.cxx )

IF (WIN32)
  SET (SSRC ${SSRC} win32/ScintillaWin.cxx win32/PlatWin.cxx)
ELSE()
  #2DO?
  SET (SSRC ${SSRC} gtk/ScintillaGTK.cxx win32/PlatGTK.cxx)
ENDIF()

ADD_LIBRARY(scintilla SHARED ${SSRC} ${DEF_FILE} ${RC_FILE})
IF (WIN32)
  TARGET_LINK_LIBRARIES(scintilla KERNEL32 USER32 GDI32 IMM32 OLE32 UUID)
ENDIF (WIN32)


SET(LSRC
  src/Editor.cxx
  src/CharClassify.cxx src/Decoration.cxx src/Document.cxx
  src/ContractionState.cxx src/CellBuffer.cxx src/CallTip.cxx
  src/PositionCache.cxx src/KeyMap.cxx src/Indicator.cxx src/LineMarker.cxx
  src/RESearch.cxx src/RunStyles.cxx src/Selection.cxx src/Style.cxx
  src/ViewStyle.cxx src/AutoComplete.cxx src/UniConversion.cxx
  src/KeyWords.cxx src/DocumentAccessor.cxx src/PropSet.cxx
  src/ExternalLexer.cxx src/StyleContext.cxx src/XPM.cxx src/PerLine.cxx
  ${LEXSRC} )

IF (WIN32)
  SET(LSRC ${LSRC} ${CMAKE_BINARY_DIR}/ScintillaBaseL.cxx ${CMAKE_BINARY_DIR}/ScintillaWinL.cxx win32/PlatWin.cxx)
ELSE ()
  #2DO? ScintillaBase.cxx?
  SET (SSRC ${SSRC} ScintillaBase.cxx gtk/ScintillaGTK.cxx win32/PlatGTK.cxx)
ENDIF ()

ADD_LIBRARY(scilexer SHARED ${LSRC} ${DEF_FILE} ${RC_FILE})
IF (WIN32)
  TARGET_LINK_LIBRARIES(scilexer KERNEL32 USER32 GDI32 IMM32 OLE32 UUID)
  SET_TARGET_PROPERTIES(scilexer PROPERTIES PREFIX "")
ENDIF (WIN32)

# ADD_LIBRARY(lexers STATIC ${LEXSRC})

SET_PROPERTY(SOURCE ${CMAKE_BINARY_DIR}/ScintillaBaseL.cxx
  APPEND PROPERTY COMPILE_DEFINITIONS SCI_LEXER)
SET_PROPERTY(SOURCE ${CMAKE_BINARY_DIR}/ScintillaWinS.cxx
  APPEND PROPERTY COMPILE_DEFINITIONS STATIC_BUILD)
SET_PROPERTY(SOURCE ${CMAKE_BINARY_DIR}/ScintillaWinL.cxx
  APPEND PROPERTY COMPILE_DEFINITIONS SCI_LEXER)

INSTALL ( TARGETS scilexer scintilla RUNTIME DESTINATION ${INSTALL_BIN} LIBRARY DESTINATION ${INSTALL_LIB} ARCHIVE DESTINATION ${INSTALL_LIB})
