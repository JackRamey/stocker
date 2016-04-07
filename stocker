#!/usr/local/bin/python

import argparse
import locale
import sys
import ystockquote

parser = argparse.ArgumentParser(description='Stock pricing tool')
parser.add_argument('-p', '--price', type=float, metavar='price', nargs='+', \
                    help="Set a theoretical price rather than use the quoted price.")
parser.add_argument('-s', '--symbol', type=str, metavar='symbol', \
                    help="Set the symbol that you wish to track.")
parser.add_argument('shares', type=int, metavar='N', nargs='+',
                    help="Number of shares.")

args = parser.parse_args()

locale.setlocale( locale.LC_ALL, '' )
symbol = 'WDAY'

if args.symbol != None:
    symbol = args.symbol

price = float(ystockquote.get_price(symbol))
show_t = False
tPrice = []

if args.price != None:
    show_t = True
    for p in args.price:
        tPrice.append(float(p))


def curr(val):
    return locale.currency(val, grouping=True)

format_str = '| {0:10} | {1:10} | {2:15} | {3:15} | {4:15} | {5:15} |' if show_t else '| {0:10} | {1:10} | {2:15} | {3:15} |'
header_str = format_str.format('Shares', 'Price', 'Value', 'Diff', '67% of Value', 'Diff') if show_t else format_str.format('Shares', 'Price', 'Value', '67% of Value')
width = len(header_str)

print('STOCKER: {0}'.format(symbol))
print('-' * width)
print(header_str)
print('-' * width)
for shares in args.shares:
    shares = int(shares)
    value = shares * price
    value_post_tax = value * .67
    if show_t:
        str1 = format_str.format(shares, curr(price), curr(value), '', curr(value_post_tax), '')
        print(str1)
        for p in tPrice:
            tValue = shares * p
            tValue_post_tax = tValue * .67
            diff1 = tValue - value
            diff2 = tValue_post_tax - value_post_tax
            sign1 = '+' if (diff1 >= 0) else ''
            sign2 = '+' if (diff2 >= 0) else ''
            str2 = format_str.format('', curr(p), curr(tValue), sign1 + curr(diff1), curr(tValue_post_tax), sign2 + curr(diff2))
            print(str2)
    else:
        print('| {0:10} | {1:10} | {2:15} | {3:15} |'.format(shares, curr(price), curr(value), curr(value_post_tax),))
    print('-' * width)


