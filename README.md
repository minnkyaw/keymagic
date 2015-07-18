# keymagic
We Now Have Official KeyMagic Website - http://keymagic.net/  KeyMagic Download Introduction      KeyMagic Unicode Keyboard Input Customizer - is a smart keyboard input method editor for complex script languanges.     With this IME, now you can have different type of Unicode keyboard layout and, you can switch to and from. As well as map/edit key layout as your own preferred type layout setting.   Features      KeyMagic program manager or application, you can manage all your compiled/generated keyboard layout.     You can generate multiple keyboad layout files, using kEditor. You will have to use Script to layout the key mapping.     Following Keyboards are pre-installed:         Myanmar3         Parabaik         myWin         Zawgyi L - Unicode         (Your Keyboard Layout Here - Please contact us)   Project Information      Programming Language : C++ / ObjC / C#     Development Status : Production/Stable     License : GNU General Public License (GPL)     Operating System : Windows 2000 / XP / Vista / 7, Mac OSX and Linux   Authors, Role and Responsibility      Thant Thet Khin Zaw     Victor, San Kho Lin   Special Thanks:      Seth N. Hetu (http://waitzar.googlecode.com) 

KeyMagic  
What is KeyMagic?
Updated Oct 18, 2011 by thantthetkz

KeyMagic is a front-end IME which aims for the languages that use complex Unicode encoding scripts (Myanmar, Khmer, Vietnamese ...). KeyMagic is used to be fast and light but much features. And supports complex patterns to customize the user inputs as you imagine.
What can it do?

Let's take look at the simplest one.

Assume I want to see "Thank you very much" on my screen when I type "TYVM". Yes, KeyMagic can do for me. I may just need to define an expression like below:

"TTVM" => "Thank you very much"

How simple is that? Expressions like this one is not actual use of KeyMagic but not limited to, you could do whatever you wanted to! ;D

Introduction

You can make keyboard layout with KeyMagic layout script language.

    Headers
    Variables
    Rules
    VirtualKeys
    String Index
    Using matched context in output
    NULL Output
    Match Any
    Not Of
    Switch
    Comments
    Escape in string
    Joining Lines 

Details
Headers

Description: Headers are used to define keyboard layout descriptions, layout settings and icon path. And they needed to be in comment block.

Syntax: @%name = %value

/*
@Name = "A Smart Unicode Keyboard Layout"
@Description = "This is keyboard description"
@TRACK_CAPSLOCK = "false"
@EAT_KEYS = "true"
@US_LAYOUT_BASED = "true"
@SMART_BACKSPACE = "true"
@TREAT_CTRL_ALT_AS_RALT = "true"
*/

    Name: Name of keyboard layout. * Implemented from v1.4
    Icon: Full path or relative path of icon file. * Implemented from v1.4
    Description: Description of the keyboard. * Implemented from v1.4
    TRACK_CAPSLOCK: Setting false to this option will ignore caps locks key. (default: true)
    SMART_BACKSPACE: Backspace will acts as undo key that will restore to the most recent state before last user input. (default: false)
    EAT_KEYS: When this option is true, any undefined keys will not have any output. (default: false)
    US_LAYOUT_BASED: Key code values will be based on US Keyboard Layout. (default: false)
    TREAT_CTRL_ALT_AS_RALT: Pressing CTRL+ALT will be treat as RALT (ALT-Gr) (default: true) 

Note: Options are case insensitive.
Variables

Dscription: Variables are used to store strings.

Syntax: $%name = %value

$vowels_cap = "AEIOU"

$vowels_small = 'aeiou'

$ka = U1000

You can also use declared variables in new variable declaration.

$vowels = $vowels_cap + $vowels_small

You can use both String and Unicode in a variable.

$AAA = "A" + U0040 + $vowles

Rules

Description: Rules are used to match the context what user type and how to customize the output.

Syntax: %pattern_to_be_match => %output_pattern

'a' => U1000

With the above rule, when user type 'a', the output will be U1000 (Myanmar က).
Virtual Key Code

Description: Virtual keys are commonly used in LHS. Anyway, they could be used in the RHS too.

Syntax: < %vkcode [ & %vkcode ] >

%vkcode can be one of the VirtualCodes.

< VK_KEY_A > => U1000

The output will be U1000 (Myanmar က) when user type 'a'.

Note: All Virtual Key inside <> need to be pressed at the same time to successfully match the pattern.

< VK_CTRL & VK_SHIFT & VK_KEY_A > => U1000

Above pattern will be matched only when the user press CONTROL KEY, SHIFT KEY and A KEY.
String index

Description: You can match one character from the string variable specifying index.

Syntax: $var[%n]

%n is the index of character.

$string = "ABCDE"
$string[1] => U1039

If you use * instead of index, it will be considered as any of character from the string array.

$string[*] => "You typed" + $1

When you used *, index of matched letter in LHS can be used in RHS.

$small = 'zxcv'
$cap = 'ZXCV'
$small[*] => $cap[$1]
// z to Z, x to X
$small[*] + $small[*] => $1 + $cap[$2]
// zx to zX, cv to cV
$small[*] + $cap[*] => $1 + $small[$2] + $2
// zZ to zzZ

Using matched context in output

Description: You can use matched context of LHS in the RHS.

Syntax: %context [ + %context ] => %n [ + %n]

'will ' + 'this ' + 'swap' => $2 + $1 + $3

When the context is will this swap then the output will be this will swap
NULL Output

Description: Set the output to empty.

Syntax: %context => null

'Blacklis' + <VK_KEY_T> => NULL // Ex1
'I need this string to delete with single backspace' + <VK_BACK> => null // Ex2

Ex1: In this example, when the context is 'Blacklis' and typing 'T' will result in empty output. Ex2: This will delete whole string as soon as you type backspace.

Note: You can still use '' or "" to have null output. 'abc' => ''
Match Any

Description: Use in the left side of the pattern. And used to match any character.

Syntax: * [+ %context] => [%pattern]

'i' + * => 'I' + $2

Not Of

Description: Used with string arrays (variables). Will match any character except from the string array.

Syntax: $var[^]

$vowels = 'aeiou'
$vowels[^] => $1 + ' is not a vowel'

Switch

Description: Switches are internal boolean(true/false) values. You could use switches as deadkey. After switch value is changed to TRUE, TRUE value only valid for next input.

Syntax: ('%switch_name')

'=' => ('myswitch') // When user type '=', switch named myswitch's value is TRUE
'z' + ('myswitch') => 'zee' // myswitch's value is now FALSE and print 'zee'
'v' + ('myswitch') => 'vee' + ('myswitch') // myswitch's value is still TRUE and print 'vee'

Comments

// : comment till the end of line.

/* */: comment multiline.
Escape in string

$slash = '\\'
$d_quote = "\""
$aUnicode = "\u1000"

Joining Lines

$vowels = 'aei' + \
          'ou'
$vowels[*] => \
            'vowles'

Comment by initrunl...@gmail.com, Oct 23, 2010

hmmm..

i have one problem. this is example of my script :

'h' => U1B33? + U1B44? 'ha' => U1B33? 'hi' => U1B33? + U1B26?

everytime i type "ha" or "hi", it will inputted as "h" (U1B33?+U1B44?). is there any mistake in my code?
Comment by project member bashr...@gmail.com, Jan 9, 2011

sorry, I didn't see your comment.

according to your script only first rule will match.

I think you want to be like the following?

'h' => U1B33? + U1B44? U1B33? + U1B44? + 'a' => U1B33? U1B33? + U1B44? + 'i' => U1B33? + U1B26? --- Results for the above script would be:
When you type 	Result
h 	U1B33? + U1B44?
ha 	U1B33?
hi 	U1B33? + U1B26?



InstallKeyMagicUbuntu  
Installing KeyMagic on Ubuntu
Updated Oct 18, 2011 by thantthetkz
deb package Released Click HERE. Try the following only if you cannot install this deb package
Introduction

About how to install KeyMagic on Ubuntu. You need to have iBus installed on your system.
Prerequisite

    Run Terminal and enter the following commands:

    sudo apt-get install libibus-dev autopoint automake
    sudo mkdir -p /usr/share/keymagic
    mkdir -p ~/.keymagic

Installing

    Download linux source archive from http://code.google.com/p/keymagic/downloads/list
    Run Terminal and enter the following commands (assuming you have downloaded file into ~/Downloads folder):

    cd ~/Downloads/
    tar -xvf ibus-keymagic-1.0.tar
    cd ibus-keymagic
    ./autogen.sh --prefix=/usr
    make
    sudo make install

Installing Keyboard Layout

    Download Keyboard Layout (example file: Download)
    Copy downloaded file

    cp "~/Downloads/Zawgyi L - Unicode.km2" "~/.keymagic/Zawgyi L - Unicode.km2"

You can either install into '/usr/shared/keymagic/' for all users
Other available keyboard layouts

    myWin
    Parabaik
    Myanmar3 

Activate Installed Keyboard

    Restart iBus
        Click on iBus icon at the panel
        Click Restart 
    Click again on iBus icon and click Preferences
    Choose 'Input Method' tab
    Select 'English->Zawgyi L - Unicode (KeyMagic)' from the combo box
    Click Add
    Click Close 

IMPORTANT TIPS:

If you run into error something like ./autogen.sh: 1: package_name: not found

Try sudo apt-get install package_name

For Example:

Says, you got an error ./autogen.sh: 1: autopoint: not found

Try sudo apt-get install autopoint

This works for most cases.


VirtualCodes  
List of Virtual Key Codes
Phase-Implementation
Updated Oct 18, 2011 by thantthetkz
Introduction

List of Virtual Key Codes used in the Keymagic Keyboard Layout Scripting.
Details

Symbolic constant name 	Value (hexadecimal) 	Keyboard equivalent
VK_BACK 	0x0008 	BACKSPACE key
VK_TAB 	0x0009 	TAB key
VK_RETURN or VK_ENTER 	0x000D 	ENTER key
VK_SHIFT 	0x0010 	SHIFT key
VK_CONTROL or VK_CTRL 	0x0011 	CTRL key
VK_ALT or VK_MENU 	0x0012 	ALT key
VK_PAUSE 	0x0013 	PAUSE key
VK_CAPITAL or VK_CAPSLOCK 	0x0014 	CAPS LOCK key
VK_KANJI 	0x0019 	JP KANJI key
VK_ESCAPE 	0x001B 	ESC key
VK_SPACE 	0x0020 	SPACEBAR
VK_PRIOR 	0x0021 	PAGE UP key
VK_NEXT 	0x0022 	PAGE DOWN key
VK_DELETE 	0x002E 	DEL key
VK_KEY_0 	0x0030 	0 key
VK_KEY_1 	0x0031 	1 key
VK_KEY_2 	0x0032 	2 key
VK_KEY_3 	0x0033 	3 key
VK_KEY_4 	0x0034 	4 key
VK_KEY_5 	0x0035 	5 key
VK_KEY_6 	0x0036 	6 key
VK_KEY_7 	0x0037 	7 key
VK_KEY_8 	0x0038 	8 key
VK_KEY_9 	0x0039 	9 key
VK_KEY_A 	0x0041 	A key
VK_KEY_B 	0x0042 	B key
VK_KEY_C 	0x0043 	C key
VK_KEY_D 	0x0044 	D key
VK_KEY_E 	0x0045 	E key
VK_KEY_F 	0x0046 	F key
VK_KEY_G 	0x0047 	G key
VK_KEY_H 	0x0048 	H key
VK_KEY_I 	0x0049 	I key
VK_KEY_J 	0x004A 	J key
VK_KEY_K 	0x004B 	K key
VK_KEY_L 	0x004C 	L key
VK_KEY_M 	0x004D 	M key
VK_KEY_N 	0x004E 	N key
VK_KEY_O 	0x004F 	O key
VK_KEY_P 	0x0050 	P key
VK_KEY_Q 	0x0051 	Q key
VK_KEY_R 	0x0052 	R key
VK_KEY_S 	0x0053 	S key
VK_KEY_T 	0x0054 	T key
VK_KEY_U 	0x0055 	U key
VK_KEY_V 	0x0056 	V key
VK_KEY_W 	0x0057 	W key
VK_KEY_X 	0x0058 	X key
VK_KEY_Y 	0x0059 	Y key
VK_KEY_Z 	0x005A 	Z key
VK_NUMPAD0 	0x0060 	Numeric keypad 0 key
VK_NUMPAD1 	0x0061 	Numeric keypad 1 key
VK_NUMPAD2 	0x0062 	Numeric keypad 2 key
VK_NUMPAD3 	0x0063 	Numeric keypad 3 key
VK_NUMPAD4 	0x0064 	Numeric keypad 4 key
VK_NUMPAD5 	0x0065 	Numeric keypad 5 key
VK_NUMPAD6 	0x0066 	Numeric keypad 6 key
VK_NUMPAD7 	0x0067 	Numeric keypad 7 key
VK_NUMPAD8 	0x0068 	Numeric keypad 8 key
VK_NUMPAD9 	0x0069 	Numeric keypad 9 key
VK_MULTIPLY 	0x006A 	Multiply key
VK_ADD 	0x006B 	Add key
VK_SEPARATOR 	0x006C 	Separator key
VK_SUBTRACT 	0x006D 	Subtract key
VK_DECIMAL 	0x006E 	Decimal key
VK_DIVIDE 	0x006F 	Divide key
VK_F1 	0x0070 	F1 key
VK_F2 	0x0071 	F2 key
VK_F3 	0x0072 	F3 key
VK_F4 	0x0073 	F4 key
VK_F5 	0x0074 	F5 key
VK_F6 	0x0075 	F6 key
VK_F7 	0x0076 	F7 key
VK_F8 	0x0077 	F8 key
VK_F9 	0x0078 	F9 key
VK_F10 	0x0079 	F10 key
VK_F11 	0x007A 	F11 key
VK_F12 	0x007B 	F12 key
VK_LSHIFT 	0x00A0 	Left Shift key
VK_RSHIFT 	0x00A1 	Right Shift key
VK_LCONTROL or VK_LCTRL 	0x00A2 	Left CTRL key
VK_RCONTROL or VK_RCTRL 	0x00A3 	Right CTRL key
VK_LMENU or VK_LALT 	0x00A4 	Left ALT key
VK_RMENU or VK_RALT 	0x00A5 	Right ALT key
VK_OEM_1 or VK_COLON 	0x00BA 	( : ; ) key
VK_OEM_PLUS 	0x00BB 	OEM_PLUS (+ =) key
VK_OEM_COMMA 	0x00BC 	OEM_COMMA (< ,) key
VK_OEM_MINUS 	0x00BD 	OEM_MINUS (_ -) key
VK_OEM_PERIOD 	0x00BE 	OEM_PERIOD (> .) key
VK_OEM_2 or VK_QUESTION 	0x00BF 	( ? / ) key
VK_OEM_3 or VK_CFLEX 	0x00C0 	( ~ ` ) key
VK_OEM_4 or VK_LBRACKET 	0x00DB 	( { [ ) key
VK_OEM_5 or VK_BACKSLASH 	0x00DC 	( | \ ) key
VK_OEM_6 or VK_RBRACKET 	0x00DD 	( } ] ) key
VK_OEM_7 or VK_QUOTE 	0x00DE 	( " ' ) key
VK_OEM_8 or VK_EXCM 	0x00DF 	( § ! ) key
VK_OEM_102 or VK_LESSTHEN 	0x00E2 	( > < ) key 
