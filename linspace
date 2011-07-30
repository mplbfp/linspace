#!/bin/bash
#
# linspace - print a sequence of floating point numbers
# usage: linspace <a> <b> <samples> [precision]

# Copyright 2011 Aaron Webster

# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.
# 
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
# 
# You should have received a copy of the GNU General Public License
# along with this program.  If not, see <http://www.gnu.org/licenses/>.

# This script is a clone of the octave/matlab function linspace, which
# produces N samples linearly distributed from a to b.  Both bounds are
# included in the sequence, so the spacing between numbers is (a-b)/(N-1).
# An optional argument, precision may be specified which controls the
# decimal digits of output precision.  The script is useful as analog to
# the coreutils function seq.  Because linspace uses bc to calculate the
# numbers, it such should be readily execuatable in all standard *nix
# environments.

# Example:
# user@unix $ linspace 1 2 10
# 1.00000000000000000000
# 1.11111111111111111111
# 1.22222222222222222222
# 1.33333333333333333333
# 1.44444444444444444444
# 1.55555555555555555556
# 1.66666666666666666667
# 1.77777777777777777778
# 1.88888888888888888889
# 2.00000000000000000000

# Builtin functions avaliable to bc may be passed as parameters as well:
# user@unix $ linspace 1 sqrt\(2\) 10
# 1.00000000000000000000
# 1.04602372915256611653
# 1.09204745830513223307
# 1.13807118745769834960
# 1.18409491661026446613
# 1.23011864576283058267
# 1.27614237491539669920
# 1.32216610406796281573
# 1.36818983322052893227
# 1.41421356237309504880

# Additionally, since bc's math library is loaded upon invocation, the
# following additional functions may be used:
# s(x) - The sine of x, x is in radians.
# c(x) - The cosine of x, x is in radians.
# a(x) - The arctangent of x, arctangent returns radians.
# l(x) - The natural logarithm of x.
# e(x) - The exponential function of raising e to the value x.
# j(n,x) - The bessel function of integer order n of x.

# Note about rounding:
# bc defaults to truncation, instead of rounding.  This script outputs the
# result, rounded to the last digit.  It does this by computing the numbers
# to twice the desired precision, then rounding to this number.

# check for proper number of command line arguments
if [ "$#" -lt 3 ]
then
	echo "linspace - return N linearly spaced numbers between a and b";
	echo "usage: linspace <a> <b> <N> [precision]"
	exit
fi

# set default precision
PRINTDIG=20
PREC=$PRINTDIG*2

# if input precision is specified, set it
if [ $4 ] 
then
	PRINTDIG=$4
	PREC=$4*2
fi

# have bc calculate the rest!
cat << EOF | bc -lq
#!/usr/bin/bc -lq 

# round function
define r (a,s) {
	auto z,f,n 
	
	n = 0;
	z=scale;
	scale=s+1;
	
	if (a < 0) { n = 1; a = -(a) }
	f=a + (5 / 10^(s + 1));
	scale=s;
	f=(f * (10^s)) / (10^s);
	if (n > 0) f = -(f);
	scale=z;
	return(f)
}

# set precision
scale=$PREC
a=$1;
b=$2;
n=$3;

# calculate spacing
if(n==1){ dn=(b-a)/n; }
if(n!=1){ dn=(b-a)/(n-1); }

# output the numbers
for(i=0;i<n;i++){
	r(a+dn*i,$PRINTDIG)
}
quit
EOF
