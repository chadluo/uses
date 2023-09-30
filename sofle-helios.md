# Sofle v2.1 RGB w/ Helios

[page history](https://github.com/chadluo/uses/commits/main/sofle-helios.md)

## Why new keybord

Anyway, impulse purchase. I've been using the [Infinity Ergodox](https://deskthority.net/wiki/Infinity_ErgoDox) (_IE_;
although out of no reason I always remembered it as Ergodox Infinity) for a while and the key between <kbd>j</kbd> and
<kbd>l</kbd> has failed mutliple times. I thought the switch was at fault and replacing it worked every time until I ran
out of spare switches. Haven't checked mechanical keyboards for quite some time I'm no longer sure about the switches,
especially what would be a good alternative to MX Red. Long story short, I went with [Gateron G Pro 3.0
Red](https://www.gateron.co/products/gateron-g-pro-3-0-switch-set) since Keychron seems to be using them. The set came
with 110 switches but I only needed a few, and given I have some spare keycaps, it's kind of intuitive to build another
keyboard just in case IE dies.

## Why Sofle

Although I'm quite happy with the IE split ortholinear layout, I wanted to check some other ergo options first. As I
said I haven't checked mechanical keyboards for a while so I must have missed quite a lot of them, but I just want to
avoid normal row-staggered keyboards. I do have a few of these keyboards and I always remap <kbd>Caps</kbd> to
<kbd>Ctrl</kbd> and <kbd>Ctrl</kbd> to <kbd>Esc</kbd>, under macOS via [Modifier
Settings](https://support.apple.com/en-au/guide/mac-help/mchlp1011/13.0/mac/13.0) and under Windows via [PowerToys
Keyboard Manager](https://learn.microsoft.com/en-us/windows/powertoys/keyboard-manager). For membrane keyboards of fixed
layout you can only change the keymapping, but for mechanical keyboards it's easier to find ones with an ergo oriented
layout.

Keychron has [Q11](https://keychron.com.au/products/keychron-q11-qmk-custom-mechanical-keyboard) which is split but
doesn't come with any more ergo design. They also have
[V10](https://keychron.com.au/products/keychron-v10-alice-layout-qmk-custom-mechanical-keyboard) and other Alice layout
models including V8. The Alice layout seems fine, but the barebone kit never came in stock in AU and it's totally
unnecessary to buy and transit from Taobao.

Then I found some local stores selling split keyboards at much higher stock level than Keychron, with popular models
including Corne, ErgoDash, Lily58 and Sofle. Corne seems too  radical for me as I wanted to keep the number row, and the
rotary encoders on Sofle feels like something fun. So far Sofle has three versions: V1, V2 and RGB (also labelled as
V2.1) with some differences in [layout and internal designs](https://josefadamcik.github.io/SofleKeyboard/#versions). I
don't need RGB but the new version probably works better I guess.

## Assembling

The [V1/V2 version build guide](https://josefadamcik.github.io/SofleKeyboard/build_guide.html) has the common steps. The
[RGB version build guide](https://josefadamcik.github.io/SofleKeyboard/build_guide_rgb.html) is mostly about the LEDs.

### Soldering

Sofle is harder to build than IE as you'll need to hand solder diodes and microcontrollers as well, especially where the
diodes are in [SMT](https://en.wikipedia.org/wiki/Surface-mount_technology) form factor. The best way should be using
[soldering paste and hot air gun soldering](https://www.youtube.com/watch?v=yNOGEtqn85o) which is probably quite close
to the industrial production procedure, but I'd like to try my soldering iron first before ordering more equipments.

I checked some tutorials and found this procedure works well for these 2-feet SMT components:

1. Melt and attach some solder to one foot
2. Keep the solder melted with the soldering iron, make sure the component direction is correct, place the it in
   position and dip the foot in the melted solder. For the hotswap socket it's easy to check the direction; for diodes
   for example in my case the bar as marked in `| T4` on the diode should align with the bar in the `|◁` sign on the
   PCB.
3. Solder the other foot.

You can do this in batches.

You can also refer to the [Reviung41 build guide](https://reviung.com/build-guide/391/) or other Reviung build guides
for microcontrollers, diodes, LEDs and hotswap sockets. Although they are different keyboards, these components are the
same and the guide comes with pictures and detailed instructions.

### Microcontroller

The website sells both [Helios](https://github.com/0xCB-dev/0xCB-Helios) and
[nice!nano](https://nicekeyboards.com/nice-nano/). The former is a Raspberri Pi based replacement for the ATmega32U4
mentioned in the build guide, and the latter seems to support wireless. I went with Helios as I don't need wireless at
the moment.

Something to notice is that Helios has 13 pins on one side instead of 12 in the [ATmega32U4 pro
micro](https://www.sparkfun.com/products/12640). If you check the pinouts you can find that `10` and `11` on the Helios
doesn't have a match on the ATmega32U4, therefore the pins should align to the far side from the USB port, leaving `10`
and `11` empty.

## Firmware

To start with, you can download the default firmware (`.uf2`) from [KEEBD](https://docs.keebd.com/firmware/). The keymap
is the one from [Sofle](https://github.com/josefadamcik/SofleKeyboard) homepage. To flash:

1. Disconnect all keyboards and connect one side without the TRRS cable. The TRRS cable is not hot unpluggable so just
   disconnect it to be safe.
2. Long press the reset key (>500ms). The keyboard should mount as a disk.
3. Copy the `.uf2` firmware file to the disk root. The keyboard should recognize the firmware, apply it and reconnect as
   a keyboard.
4. Disconnect this side, connect the other side and flash it the same way.
5. When bots sides are flashed, connect the TRRS cable and connect to the computer.

Apparently this keyboard only supports using the left hand side as main and right hand side as sub. If you connect the
right hand side to the computer it will be in a mirror flipped state where <kbd>Q</kbd> becomes <kbd>P</kbd> etc.

Helios naturally comes with QMK. If you want to configure and build your own firmware you need to have the QMK
environment (for example QMK MSYS for Windows) and you probably don't need the QMK Toolbox.

Working with QMK, to create a firmware file:

1. Follow the docs and set up the local environment, make sure the `qmk` command works.
2. Run `qmk compile -kb sofle -km default -e CONVERT_TO=helios`
3. If succeeded, in `.build/` directory there should be a `sofle_rev1_default_helios.uf2` firmware file.
4. Flash the `.uf2` firmware the same way as mentioned above.

To properly manage your layout and firmware it's probably better to follow the guide to create your own fork and create
your own branch. You can sync the main branch from upstream and merge it to your branch to keep up with upstream changes
without breaking your own. Most of your changes should be only within files at `keyboards/sofle/keymaps/[your key map]`.

## Miryoku

Docs can be found at both [manna-harbour/miryoku](https://github.com/manna-harbour/miryoku/tree/master/docs/reference)
and [manna-harbour/miryoku_qmk](https://github.com/manna-harbour/miryoku_qmk/tree/miryoku/users/manna-harbour_miryoku).

It's available to build Miryoku directly from the main qmk repo, running `qmk compile -kb sofle -km
manna-harbour_miryoku -e CONVERT_TO=helios` and collect the firmware from the same location, but some config including
base alpha layout will also be nice to have. For ease of management, edit
[manna-harbour_miryoku/config.h](https://github.com/qmk/qmk_firmware/blob/master/keyboards/sofle/keymaps/manna-harbour_miryoku/config.h)
or add a `rules.mk` in the same directory for relevant configs.