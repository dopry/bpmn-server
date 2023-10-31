bpmn-server
===========

[![Project Status: Active - The project has reached a stable, usable state and is being actively developed.](http://www.repostatus.org/badges/latest/active.svg)](http://www.repostatus.org/#active)

## Introduction
BPMN 2.0 Modeling, Execution and Presistence, an open source Workflow Server for Node.js

This package does not provide a web interface to the server. See the bpmn-server-web for a more complete web enabled version.

This package is designed specifically for Node.js and TypeScript


### Documentation
- [Featuers](./docs/features)
- [Examples](./docs/examples.md)

## Features

### Full BPMN Process Engine

bpmn-server provides an bpmnEngine to execute your workflow definition supporting all of BPMN 2.0 elements with advanced extensions

bpmn-server is highly scalable solution, allow you to run multiple nodeJS either in same machine or in a distributed mode against same MongoDB

### Presistent Processes

provides an environment to presist execution Instances while running and communicate with your application.

Applications can monitor and communicate to Instances whether they are running or offline, allowing user interface to query and process Workflow steps

### Data Queries

Since instances are saved in MongoDB you can easily query your instances (running or completed)

a full demo site is available @ http://bpmn.omniworkflow.com

# Example Script

```javascript
    const server = new BPMNServer(configuration, logger);

    let response = await server.execute('Buy Used Car');

    // let us get the items
    const items = response.items.filter(item => {
        return (item.status == 'wait');
    });

    items.forEach(item => {
        console.log(`  waiting for <${item.name}> -<${item.elementId}> id: <${item.id}> `);
    });

    console.log('Invoking Buy');

    response = await server.invoke({instanceId: response.execution.id, elementId: 'task_Buy' },
        { model: 'Thunderbird', needsRepairs: false, needsCleaning: false });

    console.log("Ready to drive");

    response = await server.invoke({ instanceId: response.execution.id, elementId: 'task_Drive' });

    console.log(`that is it!, process is now complete status=<${response.execution.status}>`)

```
for more complete examples see [Examples](./docs/examples.md)

# License

This project is licensed under the terms of the MIT license.

# Acknowledgments

The **bpmn-server** resides upon the excellent library [bpmn-io/bpmn-moddle](https://github.com/bpmn-io/bpmn-moddle) developed by [bpmn.io](http://bpmn.io/)
The **bpmn-server** is inspired by the library [bpmn-engine](https://github.com/paed01/bpmn-engine)
