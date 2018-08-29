# How to deploy your react app to digital ocean
This guide assumes you already have a local react app spun up. And that you already have a digital ocean ubuntu server setup with a non root user.

## Build your app
1. In the directory that holds your local code run `npm run build`
1. Comfirm that a build directory was created in your local directory

## Push build folder 
1. From the same directory run 
```
rsync -avz ./build {user}@:{digitaloceanip}:/home/{user}
```
Substituting the items in {} above with the values for your digital ocean server

## Setup nginx
1. SSH into your server and run `sudo nano /etc/nginx/sites-enabled/default`
1. Find the line that looks like this
```
        # include snippets/snakeoil.conf;
        root /mnt/volume-nyc1-01/html;
```
1. and change it to look like
```
        # include snippets/snakeoil.conf;
        root /home/{user}/build;
```

1. Check for nginx syntax errors with `sudo nginx -t`
1. If the output contined no errors you can restart your nginx server with `sudo systemctl restart nginx`

Everything should now work and if you go to your ip address of the server or the domain you have linked to it, you should now see your react script.

## updating your script
1. Run npm build again and run the command to push up the files again
1. If the page does not automatically get the changes you may need to ssh into your server again and restart nginx