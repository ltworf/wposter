#!/usr/bin/env python
# -*- coding: utf-8 -*-

# wposter
# Copyright (C) 2012  Salvo "LtWorf" Tomaselli
# 
# wposter is free software: you can redistribute it and/or modify
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
# 
# author Salvo "LtWorf" Tomaselli <tiposchi@tiscali.it>

import wordpresslib
import sys
import subprocess
import os
from configobj import ConfigObj

def load_conf():
    try:
        config = ConfigObj("%s/.wposter"% os.getenv("HOME"))
        wordpress = config['url']
        user = config['user']
        password = config['password']
    except:
        sys.stderr.write("Unable to load \"%s/.wposter\". Please create it and retry\n" % os.getenv("HOME"))
        sys.exit(1)
    return wordpress,user,password

def get_wordpress(b_id=0):
    wordpress,user,password=load_conf()
    wp = wordpresslib.WordPressClient(wordpress, user, password)
    
    # select blog id
    wp.selectBlog(b_id)
    return wp

def read_long_text(descr):
    
    tempfile=os.tmpnam()
    process = subprocess.Popen('%s %s'%(os.getenv("EDITOR","/usr/bin/editor"),tempfile), shell=True)
    process.wait()
    print process.returncode
    
    f=open(tempfile)
    text=f.read()
    f.close()
    return text

def write_post():
    post = wordpresslib.WordPressPost()
    post.title = raw_input('Title: ')
    
    post.description=read_long_text('Body: ')
    
    if raw_input('Confirm? [y/N] ') != 'y':
        sys.exit(0)
    return post

get_wordpress().newPost(write_post(),True)
