project('klystrack', 'c', version: '1.7.8')

sdl2 = dependency('SDL2')
sdl2_image = dependency('SDL2_image')
alsa = dependency('alsa')

res_path = get_option('prefix')+'/lib/klystrack'

add_project_arguments([
  '-DRES_PATH='+res_path,
  '-DNOSDL_MIXER',
  '-DUSESDLMUTEXES',
  '-DENABLEAUDIODUMP',
  '-DSTEREOOUTPUT',
  '-DUSESDL_IMAGE',
  '-DMIDI',
  '-DUSESDL_RWOPS',
  '-DCONFIG_DEFAULT_DISABLE_RENDER_TO_TEXTURE',
  '-DDEBUG',
  '-DCONFIG_PATH=~/.klystrack'],
  language : 'c')

makebundle = executable('makebundle', 'klystron/tools/makebundle/makebundle.c',
  include_directories : include_directories('klystron/src'),
  dependencies : [sdl2])

themes = [
  'AHX',
  'Blacklyst',
  'C64',
  'Classic',
  'Default',
  'Gameboy',
  'Golden_Brown',
  'Rust_Camo',
  'Rust_Red',
  'Ultimate_Proctamed']
foreach theme : themes
  custom_target(
    theme,
    output : theme,
    command : ['build-aux/build-theme', makebundle, '@OUTPUT@', '@SOURCE_ROOT@/themes/'+theme],
    install : true,
    install_dir : res_path+'/res')
endforeach

klystron = static_library('klystron', [
  'klystron/src/gfx/background.c',
  'klystron/src/gfx/bgcell.c',
  'klystron/src/gfx/font.c',
  'klystron/src/gfx/gfx.c',
  'klystron/src/gfx/levelbase.c',
  'klystron/src/gfx/objhdr.c',
  'klystron/src/gui/bevel.c',
  'klystron/src/gui/dialog.c',
  'klystron/src/gui/filebox.c',
  'klystron/src/gui/menu.c',
  'klystron/src/gui/mouse.c',
  'klystron/src/gui/msgbox.c',
  'klystron/src/gui/shortcuts.c',
  'klystron/src/gui/slider.c',
  'klystron/src/gui/toolutil.c',
  'klystron/src/gui/view.c',
  'klystron/src/snd/cyd.c',
  'klystron/src/snd/cydchr.c',
  'klystron/src/snd/cydcrush.c',
  'klystron/src/snd/cydentry.c',
  'klystron/src/snd/cydflt.c',
  'klystron/src/snd/cydfm.c',
  'klystron/src/snd/cydfx.c',
  'klystron/src/snd/cydosc.c',
  'klystron/src/snd/cydrvb.c',
  'klystron/src/snd/cydwave.c',
  'klystron/src/snd/freqs.c',
  'klystron/src/snd/music.c',
  'klystron/src/snd/pack.c',
  'klystron/src/util/bundle.c',
  'klystron/src/util/rnd.c'],
  include_directories : include_directories('klystron/src'),
  dependencies : [sdl2, sdl2_image])

klystrack = executable('klystrack', [
  'src/action.c',
  'src/clipboard.c',
  'src/command.c',
  'src/config.c',
  'src/console.c',
  'src/copypaste.c',
  'src/diskop.c',
  'src/edit.c',
  'src/event.c',
  'src/export.c',
  'src/help.c',
  'src/import/ahx.c',
  'src/import/hubbard.c',
  'src/import/hubdialog.c',
  'src/import/import.c',
  'src/import/mod.c',
  'src/import/org.c',
  'src/import/xm.c',
  'src/key.c',
  'src/keytab.c',
  'src/main.c',
  'src/memwriter.c',
  'src/menudefs.c',
  'src/midi.c',
  'src/mused.c',
  'src/nostalgy.c',
  'src/optimize.c',
  'src/shortcutdefs.c',
  'src/stats.c',
  'src/theme.c',
  'src/undo.c',
  'src/view/pattern.c',
  'src/view/sequence.c',
  'src/view/timer.c',
  'src/view/visu.c',
  'src/view/wavetableview.c',
  'src/view.c',
  'src/wave.c',
  'src/wave_action.c',
  'src/wavegen.c',
  'src/wavewriter.c',
  'src/zap.c',
  custom_target('gen-version-headers',
    output: ['version.h','version_number.h'],
    input: ['src/version'],
    command: ['build-aux/gen-version-headers', '@OUTDIR@', '@INPUT@'])],
  include_directories : include_directories('src', 'klystron/src'),
  dependencies : [sdl2, alsa],
  link_with : klystron,
  install : true)

executable('player', 'player/player.c',
  include_directories: include_directories('klystron/src'),
  link_with : klystron,
  dependencies: [sdl2])

executable('scancode-tool', 'scancode-tool/scancode.c',
  dependencies: [sdl2])

install_data('linux/klystrack.desktop', install_dir : 'share/applications')
install_data('icon/256x256.png', install_dir : 'share/icons/hicolor/256x256/apps', rename : 'klystrack.png')
install_data([
  'key/AZERTY',
  'key/DVORAK',
  'key/FT2',
  'key/QWERTZ',
  'key/n00bstar'],
  install_dir : res_path+'/key')
install_man('doc/klystrack.1')
install_subdir('examples', install_dir : 'share/klystrack')
