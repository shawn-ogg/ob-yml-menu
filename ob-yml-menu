#!/usr/bin/env python3

import argparse
import sys
import yaml
from string import Template

def main():
    parser = argparse.ArgumentParser(description='Convert yml file to openbox xml format')
    parser.add_argument('yml_file', help="openbox menu in yml format", type=str)
    parser.add_argument('--terminal', help="default terminal to use (defaults to xterm if not set)", type=str,
                        default='xterm')
    args = parser.parse_args()

    with open(args.yml_file, 'r') as stream:
        try:
            menu_dict = yaml.safe_load(stream)
        except yaml.YAMLError as exc:
            sys.stderr.write(str(exc))
            exit(1)

        build_xml_menu(menu_dict, args.terminal).parse_yml()

class build_xml_menu:
    indent = 0
    id_counter = 0

    def __init__(self, menu, terminal):
        try:
            menu['openbox_menu']
        except KeyError:
            sys.stderr.write("Invalid yml file (Hint: top node has to be named 'openbox_menu').")
            exit(1)
        self.menu = menu['openbox_menu']
        self.terminal = terminal

    def parse_yml(self):
        self.openbox_menu_header()
        self.build_xml()
        self.openbox_menu_footer()

    def build_xml(self, menu = None):
        if menu is None:
            menu = self.menu
        if isinstance(menu, dict):
            for key, value in menu.items():
                if value is None:
                    value = key.lower()  # for lazy people

                if key == 'separator' or key == 'sep':
                    self.print_xml('<separator />')
                elif isinstance(value, list):
                    self.print_xml(f'<menu id="{self.id_counter}" label="{key}">')
                    self.id_counter += 1
                    self.indent += 1
                    self.build_xml(value)
                    self.indent -= 1
                    self.print_xml('</menu>')
                elif '\n' in value:
                    self.print_xml(f'<menu id="{self.id_counter}" label="{key}" execute="{value.strip()}" />')
                    self.id_counter += 1
                else:
                    self.print_xml(f'<item label="{key}"> <action name="Execute">')
                    self.print_xml(f'\t<execute>{value}</execute>')
                    self.print_xml('</action></item>')
        elif isinstance(menu, list):
            for entry in menu:
                self.build_xml(entry)

    def print_xml(self, line):
        assert self.indent >= 0, "indent is negative. This is a bug!"
        print(" " * self.indent * 2, end="")
        print(Template(line).safe_substitute(terminal=self.terminal))

    def openbox_menu_header(self):
        self.print_xml('<?xml version="1.0" encoding="UTF-8"?>')
        self.print_xml('<openbox_menu>')
        self.indent += 1
        self.print_xml('<menu id="root-menu" label="Openbox 3">')
        self.indent += 1

    def openbox_menu_footer(self):
        self.indent -= 1
        self.print_xml('</menu>')
        self.indent -= 1
        self.print_xml('</openbox_menu>')


if __name__ == '__main__':
    main()

