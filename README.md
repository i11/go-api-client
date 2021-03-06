# Go API Client Gem

This gem provides access to the [ThoughtWorks Studios Go](http://www.thoughtworks-studios.com/go-continuous-delivery) API, it is capable of parsing out the atom feed and generate an object graph with all the pipelines/stages/jobs and committer information.

[![Build Status](https://api.travis-ci.org/unibet/go-api-client.svg)](https://travis-ci.org/unibet/go-api-client)
[![Gem Version](https://badge.fury.io/rb/go_api_client.svg)](http://badge.fury.io/rb/go_api_client)

## Installation

```
sudo gem install go_api_client
```

## Usage

```
require 'go_api_client'

client = GoApiClient::Client.new({:host => 'go.example.com'})
# check if the server is building 'MyProject' pipeline
! client.api(:cctray).projects({:pipeline_name => 'MyProject', :activity => 'Building'}).empty?

# check if the build has finished
client.api(:cctray).projects({:pipeline_name => 'MyProject', :activity => 'Building'}).empty?

# schedule a pipeline
client.api(:pipeline).schedule({:pipeline_name => 'MyProject'})

# Eager fetch pipeline information with parsed stages and jobs
client.api(:pipeline).pipeline({:pipeline_name => 'MyProject', :pipeline_id => 1, :eager_parser => [:stage, :job]})

# Lazy fetch pipeline and its first stage
pipeline = client.api(:pipeline).pipeline({:pipeline_name => 'MyProject', :pipeline_id => 1})
client.api(:stage).stage({:stage_uri => pipeline.stages.first})

# Eager get feed for a pipeline tougher with related stages and jobs
client.api(:feed).feed({:pipeline_name => 'MyProject', :eager_parser => [:pipeline, :stage, :job]})

# Get all properties for a given job
client.api(:properties).properties({:pipeline_name => 'MyProject', :pipeline_counter => 1, :stage_name => 'defaultStage', :stage_counter => 1, :job_name => 'defaultJob'})

# Get value of a specific property for a given job
client.api(:properties).properties({:pipeline_name => 'MyProject', :pipeline_counter => 1, :stage_name => 'defaultStage', :stage_counter => 1, :job_name => 'defaultJob', :property_name => 'cruise_agent'})

# Create a property for a given job
client.api(:properties).create_property({:pipeline_name => 'MyProject', :pipeline_counter => 1, :stage_name => 'defaultStage', :stage_counter => 1, :job_name => 'defaultJob', :property_name => 'myproperty', :property_value => 'Showcase_for_I29'})

# Get configuration of 'MyProject' pipeline
client.api(:config).pipelines({:pipeline_name => 'MyProject'})

# Get configuration of 'MyProject' pipeline eagerly parsing the template
client.api(:config).pipelines({:pipeline_name => 'MyProject', :eager_parser => [:template]})

# Get configuration for all pipelines
client.api(:config).pipelines

# Get configuration of a template
client.api(:config).templates({:template_name => 'MyTemplate'})
```

## License

Go API Client Gem is MIT Licensed

The MIT License

Copyright (c) 2014 North Development AB (http://www.unibet.com)
Copyright (c) 2012 ThoughtWorks, Inc. (http://thoughtworks.com)

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
