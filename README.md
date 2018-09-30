# NAME

Catalyst::Plugin::Shorten - The great ancient URI shortner!
========================
[![Build Status](https://travis-ci.org/ThisUsedToBeAnEmail/Catalyst-Plugin-Shorten.svg?branch=master)](https://travis-ci.org/ThisUsedToBeAnEmail/Catalyst-Plugin-Shorten)
[![Coverage Status](https://coveralls.io/repos/ThisUsedToBeAnEmail/Catalyst-Plugin-Shorten/badge.svg?branch=master)](https://coveralls.io/r/ThisUsedToBeAnEmail/Catalyst-Plugin-Shorten?branch=master)
[![CPAN version](https://badge.fury.io/pl/Catalyst-Plugin-Shorten.svg)](https://metacpan.org/pod/Catalyst-Plugin-Shorten)


# VERSION

Version 0.01

# SYNOPSIS

        use Catalyst qw/
                Shorten
                Shorten::Store::Dummy
        /;

        sub auto :Path :Args(0) {
                my ($self, $c) = @_;
                $c->shorten_extract; # checks whether the shorten param exists if it does merges the stored params into the request
        }
        
        ........

        sub endpoint :Chained('base') :PathPart('ending') :Args('0') {
                my ($self, $c) = @_;

                my $str = $c->shorten(); # returns bijection references to an ID in the store.
                my $url = $c->shorten(as_uri => 1); # return a url to the current endpoint replacing all params with localhost:300/ending?s=GH
        }
        
        -------

        use Catalyst qw/
                Shorten
                Shorten::Store::Dummy
        /;
        
        __PACKAGE__->config(
                ......
                'Plugin::Shorten' => {
                        set => [qw/c b a ..../],
                        map => {
                                params => 'data',
                                uri => 'url',
                                s => 'g'
                        }
                }
        );

        package TestApp::Controller::Shorten;

        use Moose;
        use namespace::autoclean;

        BEGIN {
                extends 'Catalyst::Controller';
        }
                
        sub g :Chained('/') :PathPart('g') :Args('1') {
                my ($self, $c, $cap) = @_;
                $c->shorten_redirect(g => $cap);
        }

        __PACKAGE__->meta->make_immutable;

        1;

# SUBROUTINES/METHODS

## shorten (as\_uri => 1|0, uri => URI, store => {} )

Take the current request uri and store, returns an Bijective string.

## shorten\_delete (s => '')

Delete from storage.

## shorten\_extract (params => { s => ...}, cb => sub)

Check for the param (default is 's'), if defined attempt to inverse and then right merge with the current requests params.

This always returns true and you can later access the merged params using - 

        $c->req->params;

## shorten\_params (params => { s => ...}, cb => sub)

Check for the param (default is 's'), if defined attempt to inverse and then return the params retrieved from storage.

## shorten\_redirect (s => '', cb => sub)

Redirect the clients browser to the uri retrieved from the storage.

## shorten\_bijection\_set (@set)

## shorten\_offset\_set ($offset)

## setup

# AUTHOR

LNATION, `<thisusedtobeanemail at gmail.com>`

# BUGS

Please report any bugs or feature requests to `bug-catalyst-plugin-shorten at rt.cpan.org`, or through
the web interface at [http://rt.cpan.org/NoAuth/ReportBug.html?Queue=Catalyst-Plugin-Shorten](http://rt.cpan.org/NoAuth/ReportBug.html?Queue=Catalyst-Plugin-Shorten).  I will be notified, and then you'll
automatically be notified of progress on your bug as I make changes.

# SUPPORT

You can find documentation for this module with the perldoc command.

    perldoc Catalyst::Plugin::Shorten

You can also look for information at:

- RT: CPAN's request tracker (report bugs here)

    [http://rt.cpan.org/NoAuth/Bugs.html?Dist=Catalyst-Plugin-Shorten](http://rt.cpan.org/NoAuth/Bugs.html?Dist=Catalyst-Plugin-Shorten)

- AnnoCPAN: Annotated CPAN documentation

    [http://annocpan.org/dist/Catalyst-Plugin-Shorten](http://annocpan.org/dist/Catalyst-Plugin-Shorten)

- CPAN Ratings

    [http://cpanratings.perl.org/d/Catalyst-Plugin-Shorten](http://cpanratings.perl.org/d/Catalyst-Plugin-Shorten)

- Search CPAN

    [http://search.cpan.org/dist/Catalyst-Plugin-Shorten/](http://search.cpan.org/dist/Catalyst-Plugin-Shorten/)

# ACKNOWLEDGEMENTS

# LICENSE AND COPYRIGHT

Copyright 2018 LNATION.

This program is free software; you can redistribute it and/or modify it
under the terms of the the Artistic License (2.0). You may obtain a
copy of the full license at:

[http://www.perlfoundation.org/artistic\_license\_2\_0](http://www.perlfoundation.org/artistic_license_2_0)

Any use, modification, and distribution of the Standard or Modified
Versions is governed by this Artistic License. By using, modifying or
distributing the Package, you accept this license. Do not use, modify,
or distribute the Package, if you do not accept this license.

If your Modified Version has been derived from a Modified Version made
by someone other than you, you are nevertheless required to ensure that
your Modified Version complies with the requirements of this license.

This license does not grant you the right to use any trademark, service
mark, tradename, or logo of the Copyright Holder.

This license includes the non-exclusive, worldwide, free-of-charge
patent license to make, have made, use, offer to sell, sell, import and
otherwise transfer the Package with respect to any patent claims
licensable by the Copyright Holder that are necessarily infringed by the
Package. If you institute patent litigation (including a cross-claim or
counterclaim) against any party alleging that the Package constitutes
direct or contributory patent infringement, then this Artistic License
to you shall terminate on the date that such litigation is filed.

Disclaimer of Warranty: THE PACKAGE IS PROVIDED BY THE COPYRIGHT HOLDER
AND CONTRIBUTORS "AS IS' AND WITHOUT ANY EXPRESS OR IMPLIED WARRANTIES.
THE IMPLIED WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR
PURPOSE, OR NON-INFRINGEMENT ARE DISCLAIMED TO THE EXTENT PERMITTED BY
YOUR LOCAL LAW. UNLESS REQUIRED BY LAW, NO COPYRIGHT HOLDER OR
CONTRIBUTOR WILL BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, OR
CONSEQUENTIAL DAMAGES ARISING IN ANY WAY OUT OF THE USE OF THE PACKAGE,
EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
