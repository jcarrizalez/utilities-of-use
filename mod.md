MOTD example with ASCII Art

```
nano /etc/motd
```

```
         _          __________                              _,
     _.-(_)._     ."          ".      .--""--.          _.-{__}-._
   .'________'.   | .--------. |    .'        '.      .:-'`____`'-:.
  [____________] /` |________| `\  /   .'``'.   \    /_.-"`_  _`"-._\
  /  / .\/. \  \|  / / .\/. \ \  ||  .'/.\/.\'.  |  /`   / .\/. \   `\
  |  \__/\__/  |\_/  \__/\__/  \_/|  : |_/\_| ;  |  |    \__/\__/    |
  \            /  \            /   \ '.\    /.' / .-\                /-.
  /'._  --  _.'\  /'._  --  _.'\   /'. `'--'` .'\/   '._-.__--__.-_.'   \
 /_   `""""`   _\/_   `""""`   _\ /_  `-./\.-'  _\'.    `""""""""`    .'`\
(__/    '|    \ _)_|           |_)_/            \__)|        '       |   |
  |_____'|_____|   \__________/   |              |;`_________'________`;-'
   '----------'    '----------'   '--------------'`--------------------`
     S T A N          K Y L E        K E N N Y         C A R T M A N
 
  ____ ____ ____ ____ ____ ____ ____ ____ _________ ____ ____ ____ ____ 
 ||M |||a |||s |||c |||a |||n |||d |||o |||       |||B |||i |||t |||s ||
 ||__|||__|||__|||__|||__|||__|||__|||__|||_______|||__|||__|||__|||__||
 |/__\|/__\|/__\|/__\|/__\|/__\|/__\|/__\|/_______\|/__\|/__\|/__\|/__\|
 
```

```
nano /etc/profile.d/wellcome.sh
```
```
#!/bin/bash
 
function color (){
  echo "\e[$1m$2\e[0m"
}
 
function extend (){
  local str="$1"
  let spaces=60-${#1}
  while [ $spaces -gt 0 ]; do
    str="$str "
    let spaces=spaces-1
  done
  echo "$str"
}
 
function center (){
  local str="$1"
  let spacesLeft=(78-${#1})/2
  let spacesRight=78-spacesLeft-${#1}
  while [ $spacesLeft -gt 0 ]; do
    str=" $str"
    let spacesLeft=spacesLeft-1
  done
   
  while [ $spacesRight -gt 0 ]; do
    str="$str "
    let spacesRight=spacesRight-1
  done
   
  echo "$str"
}
 
borderColor=35
headerLeafColor=32
headerRaspberryColor=31
greetingsColor=36
statsLabelColor=33
 
borderLine="━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
borderTopLine=$(color $borderColor "┏$borderLine┓")
borderBottomLine=$(color $borderColor "┗$borderLine┛")
borderBar=$(color $borderColor "┃")
borderEmptyLine="$borderBar                                                                              $borderBar"
 
me=$(whoami)
 
header="$borderTopLine"
 
# Greetings
greetings="$borderBar$(color $greetingsColor "$(center "Welcome back, $me!")")$borderBar\n"
greetings="$greetings$borderBar$(color $greetingsColor "$(center "$(date +"%A, %d %B %Y, %T")")")$borderBar"
 
# Print motd
echo -e "$header\n$borderEmptyLine\n$greetings\n$borderEmptyLine\n$borderBottomLine"

```
