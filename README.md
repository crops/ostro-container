Ostro Build Container
========================
This repo is to create an image that is able to setup and use an ostro build environment.

Running the container
---------------------
* **Determine the workdir**

  The workdir you create will be used for all output from the ostro build
  system as well as where your workspace will be saved. You can either
  create a new directory or use an existing one.

  *It is important that you are the owner of the directory.* The owner of the
  directory is what determines the user id used inside the container. If you
  are not the owner of the directory, you may not have access to the files
  the container creates.

  For the rest of the instructions we'll assume the workdir chosen was
  `/home/myuser/workdir`.

* **The docker command**

  Assuming you used the *workdir* from above, the command
  to run a container for the first time would be:

  ```
  docker run --rm -it -v /home/myuser/workdir:/workdir crops/ostro-container \
  --git http://some/git/repo.git
  ```
  Let's discuss some of the options:
  * **-v /home/myuser/workdir:/workdir_**: The default location of the workdir
    inside of the container is /workdir. So this part of the command says to
    use */home/myuser/workdir* as */workdir* inside the container.
  * **--git http://some/git/repo.git**: This is the
    url of the ostro metadata git repo. The metadata will automatically be
    downloaded and prepared to use inside of the workdir. Substitute in the
    url for whatever ostro git repo you want to use.

  You should see output similar to the following:
  ```
  Attempting to clone https://github.com/ostroproject/ostro-os.git
  Cloning into '/workdir'...
  remote: Counting objects: 409306, done.
  remote: Compressing objects: 100% (3289/3289), done.
  remote: Total 409306 (delta 1937), reused 26 (delta 26), pack-reused 405928
  Receiving objects: 100% (409306/409306), 148.18 MiB | 210.00 KiB/s, done.
  Resolving deltas: 100% (265525/265525), done.
  Checking connectivity... done.
  ostrouser@a2968550e11d:/workdir$

  ```
  At this point you should be able to use the shell to build ostro images.

* **Using a previous workdir**

  In the case where you have previously cloned the ostro git repo you will
  no longer need to specify the *--git* argument when starting the container.

  So the following command:
  ```
  docker run --rm -it -v /home/myuser/workdir:/workdir crops/ostro-container
  ```
  on a previously setup workdir, should generate output similar to:
  ```
  ostrouser@a2968550e11d:/workdir$
  ```

Building the container image
----------------------------
If for some reason you want to build your own image rather than using the one
on dockerhub, then run the command below in the directory containing the
Dockerfile:

```
docker build -t crops/ostro-container .
```

The argument to `-t` can be whatever you choose.
