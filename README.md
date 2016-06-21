# GenMQTT

[![Hex.pm](https://img.shields.io/hexpm/l/gen_mqtt.svg "Apache 2.0 Licensed")](https://github.com/gausby/gen_mqtt/blob/master/LICENSE)
[![Hex version](https://img.shields.io/hexpm/v/gen_mqtt.svg "Hex version")](https://hex.pm/packages/gen_mqtt)

An Elixir behaviour for communicating with an MQTT broker.

## Introduction

    ...


## Installation

Add the following to `mix.exs`:

```elixir
  defp deps do
    [{:vmq_commons, github: "erlio/vmq_commons", manager: :rebar3},
     {:gen_mqtt, "~> 0.2.0"}]
  end
```

**Note** you need to fetch the `:vmq_commons` package from GitHub for this module to work for now. `vmq_commons` should be hopefully added to Hex soon, so this step can be omitted.


## Example

The following example subscribes to a topic and responds by printing
messages.

    defmodule TemperatureLogger do
      use GenMQTT

      def start_link do
        GenMQTT.start_link(__MODULE__, nil)
      end

      def on_connect(state) do
        :ok = GenMQTT.subscribe(self, "room/+/temp", 0)
        {:ok, state}
      end

      def on_publish(["room", location, "temp"], message, state) do
        IO.puts "It is #{message} degrees in #{location}"
        {:ok, state}
      end
    end

This will log to the console every time a sensor posts a temperature
to the `room/<location/temp` topic.

It assumes an MQTT server running on localhost on port 1883.

## Versioning

Also notice, this project should follow [Semantic Versioning 2.0.0](http://semver.org), so you should be safe if you fix the version number to a specific major or minor version. The project might change, if something can be done smarter or if the underlying `:gen_emqtt` implementation changes radically.


## Legal stuff

This project rely on work done by [Erlio GmbH Basel Switzerland](http://erl.io), in specific the dependency [vmq_commons](https://github.com/erlio/vmq_commons/) which is released under an [Apache 2.0](https://github.com/erlio/vmq_commons/blob/master/LICENSE.txt)-license. This project, GenMQTT, is simply a wrapper on top of this work, and thus claim no copyright or ownership to any properties that belong to Erlio.

**Please note**: Being a third party module all bug reports found in it should be raised directly in the [issues on this projects GitHub page](https://github.com/gausby/gen_mqtt/issues), unless it is a issue found in vmq_commons.


### License

Copyright 2016 Martin Gausby

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.
