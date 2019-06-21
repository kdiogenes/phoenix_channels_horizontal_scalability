# PhoenixChannelsHorizontalScalability

There are some articles that talks how Phoenix can be used to scale websockets horizontally, but I didn't find them very
pragmatic. The most useful I find was https://gist.github.com/cerdiogenes/9405bafebdcfeb6e93ddb95d45d4d2fb and
https://www.poeticoding.com/distributed-phoenix-chat-with-pubsub-pg2-adapter/

The first one didn't use assets, but the default assets that `phx.new` generates are wonderful to play with channels,
they already implements some features for
[Fault Tolerance and Reliability Guarantees](https://hexdocs.pm/phoenix/channels.html#handling-reconnection).

The second one does a wonderful job in explaining how the differents phoenix nodes communicate, but the way is too much
manual for my taste. I just found a simplest way to connect my nodes in an article from 2016 :scream::
https://dockyard.com/blog/2016/01/28/running-elixir-and-phoenix-projects-on-a-cluster-of-nodes

So, what do you have in this repo?

  * HAProxy configured to load-balance betwen 3 HTTP servers and 3 WebSocket (WS) servers
  * Phoenix application created with --no-ecto and configured with the example channel from https://hexdocs.pm/phoenix/channels.html

How to start everything?

  * docker-compose up
  * elixir --name n1@127.0.0.1 --erl "-config sys.config" -S mix phx.server
  * PORT=4100 elixir --name n2@127.0.0.1 --erl "-config sys.config" -S mix phx.server
  * PORT=4200 elixir --name n3@127.0.0.1 --erl "-config sys.config" -S mix phx.server

Now you can visit [`localhost`](http://localhost) from your browser and start chatting. Open other tabs and you will see
that each tab will connect to different hosts and everybody is still linked, receiving the messages sended by any of the
tabs.

You can kill any node that have WS connections and the client will take care of reconecting.

## Next steps

  * Uses [phoenix-libcluster](https://github.com/manjufy/phoenix-libcluster) to make cluster formation dynamic
  * Implement auto-scaling rules and a test load script
  * Uses a more complex authentication schema for WSes

## Learn more

  * Official website: http://www.phoenixframework.org/
  * Guides: https://hexdocs.pm/phoenix/overview.html
  * Docs: https://hexdocs.pm/phoenix
  * Mailing list: http://groups.google.com/group/phoenix-talk
  * Source: https://github.com/phoenixframework/phoenix
