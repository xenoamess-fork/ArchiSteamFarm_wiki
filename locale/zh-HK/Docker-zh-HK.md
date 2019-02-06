# Docker

從版本3.0.3.2 開始, ASF 現在也可使用 **[ docker container](https://www.docker.com/what-container)**。 在 docker 容器中運行 ASF 通常對於臨時使用者沒有任何好處, 但它可能是在伺服器上使用 ASF 的絕佳方式, 確保 ASF在沙盒環境中運行, 與其他所有應用程式分離。 我們的 docker repo 可以在 **[ 此處 ](https://hub.docker.com/r/justarchi/archisteamfarm)** 找到。

* * *

## 標籤

ASF 有4種主要類型的 **[ 標籤 ](https://hub.docker.com/r/justarchi/archisteamfarm/tags)**：

### `master`

此標記始終指向從主分支中的最新提交生成的 ASF, 其工作原理與 **[ 發佈週期 ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle)** 中描述的實驗 AppVeyor 生成相同。 通常, 您應該避免此標記, 因為它有大量的漏洞，是以開發為目的專們給開發人員和高級使用者。 這映像檔會隨這每次 commit 至 GitHub 分支時更新，因此可以預期他會常常更新（以及有些東西損壞），就像我們的 AppVeyor 構建一樣。 他是標記 ASF 項目的當前狀態，不一定保證穩定或測試，就像在我們的發布週期中指出的那樣。 此標記不應在任何生產環境中使用。

### `released`

Very similar to the above, this tag always points to the latest **[released](https://github.com/JustArchiNET/ArchiSteamFarm/releases)** ASF version, including pre-releases. Compared to `master` tag, this image is being updated each time a new GitHub tag is pushed. Dedicated to advanced/power users that love to live on the edge of what can be considered stable and fresh at the same time. This is what we'd recommend if you don't want to use `latest` tag. Please note that using this tag is equal to using our **[pre-releases](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle)**.

### `latest`

This tag in comparison with previous two, as the first one includes ASF auto-updates feature and will typically point to the one of the stable versions, but not necessarily the latest one. The objective of this tag is to provide a sane default Docker container that is capable of running self-updating ASF. Because of that, the image doesn't have to be updated as often as possible, as included ASF version will always be capable of updating itself if needed. Of course, `UpdatePeriod` can be safely turned off (set to `0`), but in this case you should probably use frozen `A.B.C.D` release instead. Likewise, you can modify default `UpdateChannel` in order to make auto-updating `released` tag instead.

### `A.B.C.D`

In comparison with above tags, this tag is completely frozen, which means that the image won't be updated once published. This works similar to our GitHub releases that are never touched after the initial release, which guarantees you stable and frozen environment. Typically you should use this tag when you want to use some specific ASF release (older than `latest`) and you don't want to use any kind of auto-updates (e.g. those offered in `latest` tag).

* * *

## Which tag is the best for me?

That depends on what you're looking for. For majority of users, `latest` tag should be the best one as it offers exactly what desktop ASF does, just in special Docker container as a service. People that are rebuilding their images quite often and would instead prefer to have ASF version tied to given release are welcome to use `released` tag. If you instead want to use some specific frozen ASF version that will never change without your clear intention, `A.B.C.D` releases are available for you as fixed ASF milestones you can always fall back to.

We generally discourage trying `master` builds, just like automated AppVeyor builds - this build is here for us to mark current state of ASF project. Nothing guarantees that such state will work properly, but of course you're more than welcome to give them a try if you're interested in ASF development.

* * *

## 架構

ASF docker image is currently available for 2 architectures - `x64` and `arm`. You can read more about them in **[compatibility](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility)** section.

Since multi-arch docker tags are still work-in-progress, builds for other architectures than default `x64` are currently available with `-{ARCH}` appended to the tag name. In other words, if you want to use `latest` tag for `arm` architecture, simply use `latest-arm`.

* * *

## 使用

For complete reference you should use **[official docker documentation](https://docs.docker.com/engine/reference/commandline/docker)**, we'll cover only basic usage in this guide, you're more than welcome to dig deeper.

### 你好，ASF！

Firstly we should verify if our docker is even working correctly, this will serve as our ASF "hello world":

```shell
docker pull justarchi/archisteamfarm
docker run -it --name asf justarchi/archisteamfarm
```

`docker pull` command ensures that you're using up-to-date `justarchi/archisteamfarm` image, just in case you had outdated local copy in your cache. `docker run` creates a new ASF docker container for you and runs it in the foreground (`-it`).

If everything ended successfully, after pulling all layers and starting container, you should notice that ASF properly started and informed us that there are no defined bots, which is good - we verified that ASF in docker works properly. Hit `CTRL+P` then `CTRL+Q` in order to quit foreground docker container, then stop ASF container with `docker stop asf`, and remove it with `docker rm asf`.

If you take a closer look at the command then you'll notice that we didn't declare any tag, which automatically defaulted to `latest` one. If you want to use other tag than `latest`, for example `latest-arm`, then you should declare it explicitly:

```shell
docker pull justarchi/archisteamfarm:latest-arm
docker run -it --name asf justarchi/archisteamfarm:latest-arm
```

* * *

## Using a volume

If you're using ASF in docker container then obviously you need to configure the program itself. You can do it in various different ways, but the recommended one would be to create ASF `config` directory on local machine, then mount it as a shared volume in ASF docker container.

For example, we'll assume that your ASF config folder is in `/home/archi/ASF/config` directory. This directory contains core `ASF.json` as well as bots that we want to run. Now all we need to do is simply attaching that directory as shared volume in our docker container, where ASF expects its config directory (`/app/config`).

```shell
docker pull justarchi/archisteamfarm
docker run -it -v /home/archi/ASF/config:/app/config --name asf justarchi/archisteamfarm
```

And that's it, now your ASF docker container will use shared directory with your local machine in read-write mode, which is everything you need for configuring ASF.

Of course, this is just one specific way to achieve what we want, nothing is stopping you from e.g. creating your own `Dockerfile` that will copy your config files into `/app/config` directory inside ASF docker container. We're only covering basic usage in this guide.

### Volume permissions

ASF is by default run with default `root` user inside a container. This is not a problem security-wise, since we're already inside Docker container, but it does affect the shared volume as newly-generated files will be normally owned by `root`, which might not be desired situation when using a shared volume.

Docker allows you to pass `--user` **[flag](https://docs.docker.com/engine/reference/run/#user)** to `docker run` command which will define default user that ASF will run under. You can check your `uid` and `gid` for example with `id` command, then pass it to the rest of the command. For example, if your target user has `uid` and `gid` of 1000:

```shell
docker pull justarchi/archisteamfarm
docker run -it -u 1000:1000 -v /home/archi/ASF/config:/app/config --name asf justarchi/archisteamfarm
```

Remember that by default `/app` directory used by ASF is still owned by `root`. If you run ASF under custom user, then your ASF process won't have write access to its own files. This access is not mandatory for operation, but it is crucial e.g. for auto-updates feature. In order to fix this, it's enough to change ownership of all ASF files from default `root` to your new custom user.

```shell
docker exec -u root asf chown -hR 1000:1000 /app
```

This has to be done only once after you created your container with `docker run`, and only if you decided to use custom user for ASF process. Also don't forget to change `1000:1000` argument in both commands above to the `uid` and `gid` you actually want to run ASF under.

* * *

## 指令行參數

ASF allows you to pass **[command-line arguments](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments)** in docker container by using `ASF_ARGS` environment variable. This can be added on top of `docker run` with `-e` switch. 範例：

```shell
docker pull justarchi/archisteamfarm
docker run -it -e "ASF_ARGS=--cryptkey MyPassword" --name asf justarchi/archisteamfarm
```

This will properly pass your `--cryptkey` argument to ASF process being run inside docker container. Of course, if you're advanced user then you can also modify `ENTRYPOINT` and pass your custom arguments yourself.

Unless you want to provide custom encryption key or other advanced options, usually you don't need to include any special `ASF_ARGS` as our docker containers are already configured to run with a sane expected default options of `--no-restart` `--process-required` `--system-required`.

* * *

## IPC

For using IPC, firstly you should switch `IPC` **[global configuration property](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#global-config)** to `true`. In addition to that, you **must** modify default listening address of `localhost`, as docker can't route outside traffic to loopback interface. An example of a setting that will listen on all interfaces would be `http://*:1242`. Of course, you can also use more restrictive bindings, such as local LAN or VPN network only, but it has to be a route accessible from the outside - `localhost` won't do, as the route is entirely within guest machine.

For doing the above you should use **[custom IPC config](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#custom-configuration)** such as the one below:

```json
{
    "Kestrel": {
        "Endpoints": {
            "HTTP": {
                "Url": "http://*:1242"
            }
        }
    }
}
```

Once we set up IPC on non-loopback interface, we need to tell docker to map ASF's `1242/tcp` port either with `-P` or `-p` switch.

For example, this command would expose ASF IPC interface to host machine (only):

```shell
docker pull justarchi/archisteamfarm
docker run -it -p 127.0.0.1:1242:1242 -p [::1]:1242:1242 --name asf justarchi/archisteamfarm
```

If you set everything properly, `docker run` command above will make **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** interface work from your host machine, on standard `localhost:1242` route that is now properly redirected to your guest machine. It's also nice to note that we do not expose this route further, so connection can be done only within docker host, and therefore keeping it secure.

* * *

### Complete example

Combining whole knowledge above, an example of a complete setup would look like this:

```shell
docker pull justarchi/archisteamfarm
docker run -it -p 127.0.0.1:1242:1242 -p [::1]:1242:1242 -v /home/archi/asf:/app/config --name asf justarchi/archisteamfarm
```

This assumes that you have all ASF config files in `/home/archi/asf`, if not, you should modify the path to the one that matches. This setup is also ready for optional IPC usage if you've decided to include `IPC.config` in your config directory with a content like below:

```json
{
    "Kestrel": {
        "Endpoints": {
            "HTTP": {
                "Url": "http://*:1242"
            }
        }
    }
}
```

* * *

## Pro tips

When you already have your ASF docker container ready, you don't have to use `docker run` every time. You can easily stop/start ASF docker container with `docker stop asf` and `docker start asf`. Keep in mind that if you're not using `latest` tag then updating ASF will still require from you to `docker stop`, `docker rm`, `docker pull` and `docker run` again. This is because you must rebuild your container from fresh ASF docker image every time you want to use ASF version included in that image. In `latest` tag, ASF has included capability to auto-update itself, so rebuilding the image is not necessary for using up-to-date ASF (but it might still be a good idea to do it from time to time in order to use fresh .NET Core runtime and underlying OS).

As hinted by above, ASF in tag other than `latest` won't automatically update itself, which means that **you** are in charge of using up-to-date `justarchi/archisteamfarm` repo. This has many advantages as typically the app should not touch its own code when being run, but we also understand convenience that comes from not having to worry about ASF version in your docker container. If you care about good practices and proper docker usage, `released` tag is what we'd suggest instead of `latest`, but if you can't be bothered with it and you just want to make ASF both work and auto-update itself, then `latest` will do.

You should typically run ASF in docker container with `Headless: true` global setting. This will clearly tell ASF that you're not here to provide missing details and it should not ask for those. Of course, for initial setup you should consider leaving that option at `false` so you can easily set up things, but in long-run you're typically not attached to ASF console, therefore it'd make sense to inform ASF about that and use `input` **[command](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** if need arises. This way ASF won't have to wait infinitely for user input that will not happen (and waste resources while doing so).