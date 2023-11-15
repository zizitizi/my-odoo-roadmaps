

# after installing ssl website editor not working

Just add this line on your nginx config file, on server block. It hangs because a request is send over http instead of https.

add_header 'Content-Security-Policy' 'upgrade-insecure-requests'; 
I tried with Odoo 16 CE, on ubuntu 22.04, and it works fine


https://www.odoo.com/forum/help-1/odoo-16-website-editor-i-cannot-edit-on-my-website-215417


























