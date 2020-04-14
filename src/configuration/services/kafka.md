# Kafka (Message Queue service)

Apache Kafka is an open-source stream-processing software platform.  It is a framework for storing, reading and analysing streaming data.

See the [Kafka documentation](https://kafka.apache.org/documentation/) for more information.

## Supported versions

* 2.1
* 2.2
* 2.3
* 2.4

## Relationship

The format exposed in the ``$PLATFORM_RELATIONSHIPS`` [environment variable](/development/variables.md#platformsh-provided-variables):

{% codesnippet "https://examples.docs.platform.sh/relationships/kafka", language="json" %}{% endcodesnippet %}

## Usage example

In your ``.platform/services.yaml``:

{% codesnippet "/registry/images/examples/full/kafka.services.yaml", language="yaml" %}{% endcodesnippet %}

In your ``.platform.app.yaml``:

{% codesnippet "/registry/images/examples/full/kafka.app.yaml", language="yaml" %}{% endcodesnippet %}

You can then use the service in a configuration file of your application with something like:

{% codetabs name="Java", type="java", url="https://examples.docs.platform.sh/java/kafka" -%}

{%- language name="Python", type="python", url="https://examples.docs.platform.sh/python/kafka" -%}

{%- language name="Ruby", type="ruby"-%}
## With the ruby-kafka gem

# Producer
require "kafka"
kafka = Kafka.new(["kafka.internal:9092"], client_id: "my-application")
kafka.deliver_message("Hello, World!", topic: "greetings")

# Consumer
kafka.each_message(topic: "greetings") do |message|
  puts message.offset, message.key, message.value
end

{%- endcodetabs %}

(The specific way to inject configuration into your application will vary. Consult your application or framework's documentation.)