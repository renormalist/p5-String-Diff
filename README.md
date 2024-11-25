# NAME

String::Diff - Simple diff to String

# SYNOPSIS

    use String::Diff;
    use String::Diff qw( diff_fully diff diff_merge diff_regexp );# export functions

    # simple diff
    my($old, $new) = String::Diff::diff('this is Perl', 'this is Ruby');
    print "$old\n";# this is [Perl]
    print "$new\n";# this is {Ruby}

    my $diff = String::Diff::diff('this is Perl', 'this is Ruby');
    print "$diff->[0]\n";# this is [Perl]
    print "$diff->[1]\n";# this is {Ruby}

    my $diff = String::Diff::diff('this is Perl', 'this is Ruby',
        remove_open => '<del>',
        remove_close => '</del>',
        append_open => '<ins>',
        append_close => '</ins>',
    );
    print "$diff->[0]\n";# this is <del>Perl</del>
    print "$diff->[1]\n";# this is <ins>Ruby</ins>

    # merged
    my $diff = String::Diff::diff_merge('this is Perl', 'this is Ruby');
    print "$diff\n";# this is [Perl]{Ruby}

    my $diff = String::Diff::diff_merge('this is Perl', 'this is Ruby',
        remove_open => '<del>',
        remove_close => '</del>',
        append_open => '<ins>',
        append_close => '</ins>',
    );
    print "$diff\n";# this is <del>Perl</del><ins>Ruby</ins>

    # change to default marks
    %String::Diff::DEFAULT_MARKS = (
        remove_open  => '<del>',
        remove_close => '</del>',
        append_open  => '<ins>',
        append_close => '</ins>',
        separator    => '&lt;-OLD|NEW-&gt;', # for diff_merge
    );

    # generated for regexp
    my $diff = String::Diff::diff_regexp('this is Perl', 'this is Ruby');
    print "$diff\n";# this\ is\ (?:Perl|Ruby)

    # detailed list
    my $diff = String::Diff::diff_fully('this is Perl', 'this is Ruby');
    for my $line (@{ $diff->[0] }) {
        print "$line->[0]: '$line->[1]'\n";
    }
    # u: 'this is '
    # -: 'Perl'

    for my $line (@{ $diff->[1] }) {
        print "$line->[0]: '$line->[1]'\n";
    }
    # u: 'this is '
    # +: 'Ruby'

# DESCRIPTION

String::Diff is the difference of a consecutive string is made.
after general diff is done, the difference in the line is searchable.

the mark of the addition and the deletion can be freely changed.
the color is colored to the terminal with ANSI, using the HTML display it.

after the line is divided, diff is taken when 'linebreak' option is specified.

    my($old_string, $new_string) = String::Diff::diff_fully('this is Perl', 'this is Ruby', linebreak => 1);
    my($old_string, $new_string) = String::Diff::diff('this is Perl', 'this is Ruby', linebreak => 1);
    my $string = String::Diff::diff_merge('this is Perl', 'this is Ruby', linebreak => 1);
    my $string = String::Diff::diff_regexp('this is Perl', 'this is Ruby', linebreak => 1);

In diff and diff\_merge methods the mark of the difference can be changed.

    my $diff = String::Diff::diff('this is Perl', 'this is Ruby',
        remove_open => '<del>',
        remove_close => '</del>',
        append_open => '<ins>',
        append_close => '</ins>',
    );

You can escape callback set to diff function and diff\_merge function.

    use HTML::Entities
    my($diff_old, $diff_new) = String::Diff::diff(
        'this is <b>Perl</b>',
        'this is <b><BIG>R</BIG>uby</b>',
        remove_open => '<del>',
        remove_close => '</del>',
        append_open => '<ins>',
        append_close => '</ins>',
        escape       => sub { encode_entities($_[0]) },
    });
    is($diff_old, 'this is &lt;b&gt;<del>Perl</del>&lt;/b&gt;');
    is($diff_new, 'this is &lt;b&gt;<ins>&lt;BIG&gt;R&lt;/BIG&gt;uby</ins>&lt;/b&gt;');

# METHODS

- diff\_fully

        the list that divides diff according to the mark is returnd.

          my($old_string, $new_string) = String::Diff::diff_fully('this is Perl', 'this is Ruby');

- diff

        abd the mark of the deletion and the addition is given to the string.

- diff\_merge

        old and new string is merged with diff.

- diff\_regexp

        the regular expression to which old string and new string are matched with regexp is returned.

# AUTHOR

Kazuhiro Osawa &lt;yappo {@} shibuya {dot} pl>

# SEE ALSO

[Algorithm::Diff](https://metacpan.org/pod/Algorithm%3A%3ADiff)

# LICENSE

Copyright 2008 (C) Kazuhiro Osawa

This library is free software; you can redistribute it and/or modify
it under the same terms as Perl itself.
