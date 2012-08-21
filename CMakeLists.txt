# Copyright (C) 2007-2012 LuaDist.
# Created by David Manura, Peter Drahos
# Redistribution and use of this file is allowed according to the terms of the MIT license.
# For details see the COPYRIGHT file distributed with LuaDist.
# Please note that the package source code is licensed under its own license.

project ( scintilla CXX )
cmake_minimum_required ( VERSION 2.8 )
include ( cmake/dist.cmake )

include_directories ( include src lexlib )
set ( SRC_BASE src/AutoComplete.cxx src/CallTip.cxx src/CellBuffer.cxx lexlib/CharacterSet.cxx src/CharClassify.cxx src/ContractionState.cxx src/Decoration.cxx src/Document.cxx src/Editor.cxx src/KeyMap.cxx src/Indicator.cxx src/LineMarker.cxx src/PerLine.cxx src/PositionCache.cxx lexlib/PropSetSimple.cxx src/RESearch.cxx src/RunStyles.cxx src/Selection.cxx src/Style.cxx src/UniConversion.cxx src/ViewStyle.cxx src/XPM.cxx
  )
set ( SRC_SCINTILLA src/ScintillaBase.cxx )
set ( SRC_LEX lexlib/Accessor.cxx src/Catalogue.cxx src/ExternalLexer.cxx lexlib/LexerBase.cxx lexlib/LexerModule.cxx lexlib/LexerSimple.cxx src/ScintillaBase.cxx lexlib/StyleContext.cxx lexlib/WordList.cxx )
file ( GLOB SRC_LEXERS lexers/*.cxx )

if ( WIN32 )
  include_directories ( win32 )
  list ( APPEND SRC_BASE win32/PlatWin.cxx )
  list ( APPEND SRC_SCINTILLA win32/ScintillaWin.cxx win32/Scintilla.def win32/ScintRes.rc )
  list ( APPEND SRC_LEX win32/ScintillaWin.cxx )
  set ( LIBS -limm32 -lole32 -luuid )
endif ()

add_library ( scilexer ${SRC_BASE} ${SRC_LEX} ${SRC_LEXERS} )
add_library ( scintilla ${SRC_BASE} ${SRC_SCINTILLA} )
target_link_libraries ( scilexer ${LIBS} )
target_link_libraries ( scintilla ${LIBS} )

install_library ( scintilla scilexer )
install_header ( include/ )
install_doc ( doc/ )
install_test ( test/ )
install_data ( README License.txt )
