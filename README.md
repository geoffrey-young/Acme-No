# NAME 

Acme::No - makes no() work the way I want it to

# SYNOPSIS

    use 5.6;            # I use our(), so 5.6 is required
    no  6.0;            # but this was coded for perl 5, not perl 6
                        # and the perl 6 compat layer isn't really 5.6
                        # so my code breaks under 6.0

    use mod_perl 1.27;  # we need at least version 1.27
    no mod_perl 2.0;    # but mod_perl 2.0 is entirely different than 1.0
                        # so keep my cpan email to a minimum
                     

# DESCRIPTION

ok, first the appropriate pod:

$ perldoc `-f` no 
  =item no Module VERSION LIST

    =item no Module VERSION

    =item no Module LIST

    =item no Module

    See the L</use> function, which C<no> is the opposite of.

now, one might think that, since 

    use mod_perl 1.27;

makes sure that mod\_perl is at least version 1.27,

    no mod_perl 1.27;

should mean that 1.27 is too high - the manpage says use() and
no() are opposites, and that looks like opposite behavior to 
me.  however...

    $ perl -e 'use mod_perl 2.0'
    mod_perl version 2 required--this is only version 1.2701 at -e line 1.
    BEGIN failed--compilation aborted at -e line 1.

    $ perl -e 'no mod_perl 2.0'
    mod_perl version 2 required--this is only version 1.2701 at -e line 1.
    BEGIN failed--compilation aborted at -e line 1.

so, no() and use() do the exact same thing here - hmmm... looks like a 
bug in perl core...

enter Acme::No

Acme::No makes no() work the way I want it to.

    $ perl -MAcme::No -e'no v5.9.0; print "ok\n"'
    Perl v5.009 too high--version less than v5.009 required at -e line 0

    $ perl -MAcme::No -e'no v5.9.1; print "ok\n"'
    ok

    $ perl -MAcme::No -e'no mod_perl 1.27; print "ok\n"'
    mod_perl version 1.2701 too high--version less than 1.27 required at -e line 0

    $ perl -MAcme::No -e'no mod_perl 2.0; print "ok\n"'
    ok

# FEATURES/BUGS

probably lots

# SEE ALSO

Filter::Util::Call, perldoc `-f` use, perldoc `-f` no,
http://www.mail-archive.com/perl5-porters@perl.org/msg53742.html,
http://www.mail-archive.com/perl5-porters@perl.org/msg53752.html,

# AUTHOR

Geoffrey Young <geoff@modperlcookbook.org>

# COPYRIGHT

Copyright (c) 2002, Geoffrey Young
All rights reserved.

This module is free software.  It may be used, redistributed
and/or modified under the same terms as Perl itself.
