# Install-and-Configure-Redis-on-Ubuntu-16.04

### Install the Build and Test Dependencies

We can update our local apt package cache and install the dependencies by typing:

```bash
sudo apt-get update
sudo apt-get install build-essential tcl
```
### Download, Compile, and Install Redis

### Download and Extract the Source Code

Since we won’t need to keep the source code that we’ll compile long term (we can always re-download it), we will build in the ```/tmp``` directory. Let's move there now:

```cd /tmp```

Now, download the latest stable version of Redis. This is always available at a stable download URL:

```curl -O http://download.redis.io/redis-stable.tar.gz```

Unpack the tarball by typing:

```tar xzvf redis-stable.tar.gz```

Move into the Redis source directory structure that was just extracted:

```cd redis-stable```

### Build and Install Redis

Since we won’t need to keep the source code that we’ll compile long term (we can always re-download it), we will build in the ```/tmp``` directory. Let's move there now:

```cd /tmp```

Now, download the latest stable version of Redis. This is always available at a stable download URL :

```curl -O http://download.redis.io/redis-stable.tar.gz```

Unpack the tarball by typing:

```tar xzvf redis-stable.tar.gz```

Move into the Redis source directory structure that was just extracted:

```cd redis-stable```

### Build and Install Redis

Now, we can compile the Redis binaries by typing:

```make```

After the binaries are compiled, run the test suite to make sure everything was built correctly. You can do this by typing:

```make test```

This will typically take a few minutes to run. Once it is complete, you can install the binaries onto the system by typing:

```sudo make install```

### Configure Redis

To start off, we need to create a configuration directory. We will use the conventional /etc/redis directory, which can be created by typing:

```sudo mkdir /etc/redis```

Now, copy over the sample Redis configuration file included in the Redis source archive:

```sudo cp /tmp/redis-stable/redis.conf /etc/redis```

Next, we can open the file to adjust a few items in the configuration:

```sudo nano /etc/redis/redis.conf```

In the file, find the supervised directive. Currently, this is set to no. Since we are running an operating system that uses the systemd init system, we can change this to systemd.

Next, find the ```/dir``` directory. This option specifies the directory that Redis will use to dump persistent data. We need to pick a location that Redis will have write permission and that isn't viewable by normal users.

We will use the ```/var/lib/redis``` directory for this, which we will create in a moment.

Create a Redis systemd Unit File

Create and open the ```/etc/systemd/system/redis.service``` file to get started.

```sudo nano /etc/systemd/system/redis.service```

Inside, we can begin the [Unit] section by adding a description and defining a requirement that networking be available before starting this service.

In the [Service] section, we need to specify the service’s behavior. For security purposes, we should not run our service as root. We should use a dedicated user and group, which we will call redis for simplicity. We will create these momentarily.

To start the service, we just need to call the redis-server binary, pointed at our configuration. To stop it, we can use the Redisshutdown command, which can be executed with the redis-cli binary. Also, since we want Redis to recover from failures when possible, we will set the Restart directive to ```“always”```.

Finally, in the [Install] section, we can define the systemd target that the service should attach to if enabled (configured to start at boot).

### Create the Redis User, Group and Directories

Now, we just have to create the user, group, and directory that we referenced in the previous two files.

Begin by creating the redis user and group. This can be done in a single command by typing:

```sudo adduser — system — group — no-create-home redis```

Now, we can create the ```/var/lib/redis``` directory by typing:

```sudo mkdir /var/lib/redis```

We should give the redis user and group ownership over this directory:

```sudo chown redis:redis /var/lib/redis```

Adjust the permissions so that regular users cannot access this location:

```sudo chmod 770 /var/lib/redis```

### Start and Test Redis

Start up the systemd service by typing:

```sudo systemctl start redis```

Check that the service had no errors by running:

```sudo systemctl status redis```

### Test the Redis Instance Functionality

To test that your service is functioning correctly, connect to the Redis server with the command-line client:

```redis-cli```

### Enable Redis to Start at Boot

If all of your tests worked, and you would like to start Redis automatically when your server boots, you can enable the systemd service.

To do so, type:

```sudo systemctl enable redis```

### Connect to Redis

Connect to with Redis Desktop Manager:

If you connect another server to Redis, you can modify to “bind” parameter; from 172.0.0.1 → 0.0.0.0.

Open your redis config file : ```sudo nano etc/redis/redis.conf```

Redis Desktop Manager This is always available at a stable download URL: https://redisdesktop.com/

Have Enjoy !
