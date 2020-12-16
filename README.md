- Django + nginx
- RTMP 라이브 스트리밍
- 여러개를 동시 전송 가능
- HLS 기반 재생 지원
- HLS AES-128 을 이용한 재생 또는 초 단위 과금 구현 가능
- 라이브 스트림 캡쳐 가능 (nginx-vod-module)
- WebVTT를 이용한 타임 메타데이터
ㅤ→ 채팅 룸 메시지 동기화
ㅤ→ 스포츠 경기 중계 동기화


# Boltstream
Self-hosted Live Video Streaming Website + Backend

Reference website: [https://boltstream.me](https://boltstream.me)

This is the result of a series of blog posts that I made [here](https://benwilber.github.io/nginx/rtmp/live/video/streaming/2018/03/25/building-a-live-video-streaming-website-part-1-start-streaming.html)

Join us on Freenode!  [#boltstream](https://kiwiirc.com/nextclient/irc.freenode.net/#boltstream)

## Features

* Live stream via RTMP with a stream key to your RTMP ingest server
* Any number of simultaneous live streams
* Playback via standard HLS
* Restrict playback with HLS segment encryption (AES-128)
	* Pay-per-view
	* Pay-per-minute :-)
* Capture VODs (recordings) of live streams
	* Live-clipping of VODs via nginx-vod-module
* Timed metadata via WebVTT
	* Synchronized chat room messages
	* Synchronized play-by-play live sports events


## Core Components

* [Django](https://www.djangoproject.com/)
	* Web application
* [nginx](https://nginx.org/)
	* Web server
* [nginx-rtmp](https://github.com/arut/nginx-rtmp-module)
	* RTMP ingest
	* Actually, we use a [fork](https://github.com/sergey-dryabzhinsky/nginx-rtmp-module) that adds a number of features that we use.  The original nginx-rtmp hasn't been updated in years.
* [nginx-vod-module](https://github.com/kaltura/nginx-vod-module)
	* HLS VODs
	* Live clipping
* [FFMPEG](https://ffmpeg.org/)
	* Various video encoding/packaging

## Optional Components

* [SportRadar](https://www.sportradar.com/)
	* Realtime sports play-by-play data synchronized to your live streams
* [ACRCloud](https://www.acrcloud.com/)
	* Audio content recognition (Shazaam for your live streams)

	
## Getting Started

Clone this repo!

There is a [Terraform](https://terraform.io/) configuration for deploying this infrastructure on [DigitalOcean](https://www.digitalocean.com/).

First, edit `ansible/site.yml` and update all the variables like `<your ...variable_here>`.

You will also need to modify some variables in `terraform/terraform.tfvars`, and then from within the `terraform` directory, just run:

```bash
$ make apply
```

At the end of the Terraform deployment (might take 10-15 minutes), you will have a full self-hosted live video streaming platform with your own RTMP ingest and playback endpoints.

Happy Streaming!

## Running Locally

If you're interested in running the Django app locallly, then you can do:

```shell
# Set up Python virtualenv
$ make venv
$ source venv/bin/activate

# Install Python dependencies
$ make deps

# Apply database migrations (db.sqlite3) and load the initial data
$ make cleandb

# Run the Django development server
$ make run
```

You can log in with the default user at:

Website|Username|Password
:--|:--|:--
[http://localhost:8000/sign-in](http://localhost:8000/sign-in)|`boltstream`|`boltstream`


## Help Wanted!

I put this stack together about a year ago and haven't been able to push much further on it.

Ideally it could be deployed in Docker (I don't know anything about Docker or Kubernetes).  The nginx and Django stuff seems like it could be pretty easy to containerize.

Help me package this up!  We need more federated live-streaming platforms!  It can't just always be Twitch.tv, YouTube, and Facebook!
