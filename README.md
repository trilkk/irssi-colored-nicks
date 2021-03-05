# irssi-colored-nicks

Colors nicknames by with a pseudo-random hash calculated from the nickname. Includes an example theme.

## Usage

Clone this repository somewhere in your home directory. Let's assume directly at `~/`:

    cd ~
    git clone https://github.com/trilkk/irssi-colored-nicks.git

Go to `.irssi/scripts/` and link the script so it can be enabled:

    cd ~/.irssi/scripts
    ln -s ../../irssi-colored-nicks/colored_nicks.pl

To enable the script automatically, add it to `~/.irssi/scripts/autorun/`:

    cd ~/.irssi/scripts/autorun
    ln -s ../colored_nicks.pl

Go to `.irssi` and link the associated theme so it can be enabled in configuration:

    cd ~/.irssi
    ln -s ../irssi-colored-nicks/colored_nicks.theme

## Settings

The script provides the following settings:

    colored_nicks_colors : str
    colored_nicks_hash_function : str
    colored_nicks_truncation_long : int
    colored_nicks_truncation_short : int

`colored_nicks_colors` is a whitespace-separated list of irssi color codes that will be used as the array of colors to use.

`colored_nicks_hash_function` is the hash function used to calculate the hash for over the nicknames. There are two hash functions available: `djb2` and `sdbm`. Any setting value other than the default `djb2` selects `sdbm` hash.

`colored_nicks_truncation_long` and `colored_nicks_truncation_short` are truncation settings in character lengths to chich the nicknames are truncated. Values equal to or smaller than `0` disable the truncation.

Example addition to `~/.irssi/config`:

    settings = {
        "fe-common/core" = {
            theme = "colored_nicks.theme";
        };
        "perl/core/scripts" = {
            colored_nicks_colors = "%c %X2L %g %X2J %X3I %w %X7P %X7S %m %X54 %X44 %X56 %X46 %B %X2B %y %X5C %X4C";
            colored_nicks_truncation_long = "12";
            colored_nicks_truncation_short = "11";
        };
    };

## Commands

Use the command `/colored_nicks_list` to display debug listing of all colorizations. Together with the `/cubes` command from `cubes.pl` it can be used to easily debug potential nickname colors and ordering.

## Themes

Due to also including truncation and padding, the expandos provided by the script embed the nicknames within the expando itself - they are not simply color codes. This allows the theme to position the user mode character before or after the nickname without worrying about the whitespace introduced by irssi if using the `$[<num>]variable` syntax.

The script provides the following expandos:

    $cnnick
    $cnpadl
    $cnpads
    $cnuser

`$cnnick` is the truncated string for general nicknames, `$cnuser` is the truncated string for the user's nickname.

`$cnpadl` is the whitespace padding remaining after truncating the nicknames into `colored_nicks_truncation_long`. `$cnpads` is the whitespace padding remaining after truncating the nicknames into `colored_nicks_truncation_short`.

Please take a look at `colored_nicks.theme` for details on how to use the expandos.

## References

**\[1\]** [cubes.pl](https://github.com/irssi/scripts.irssi.org/blob/master/scripts/cubes.pl) can be used to get a nice listing of all irssi color codes.

**\[2\]** [nickcolor-expando.pl](https://github.com/irssi/scripts.irssi.org/blob/master/scripts/nickcolor_expando.pl) by **Nei** is a different approach at nick coloring. This script borrows the irssi color code to terminal color code tranformation from said script.
