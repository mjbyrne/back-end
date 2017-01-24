var http = require("http"); 
var server = http.createServer(function(request, response) { 
	response.writeHead(200, {"Content-Type": "text/html", "Access-control-allow-origin": "*"}); 
	response.write("<h1>Hello!</h1><p>You asked for <code>" + request.url + "</code></p>"); 
	response.end(); 
}); 

var  clientList = []; 
server.on('connection', function(client) { 
	client.name = client.remoteAddress + ":" + client.remotePort; 
	client.write('Chat Server : Welcome to Chat, ' + client.name + '\n');
	clientList.push(client);
	broadcast(client.name + "entered the chat room.\n", {name: "Chat Server"});
	client.on('data' , function(data) {
		//console.log(data);
		broadcast(data, client);
	});

	client.on('end', function(data){
		//console.log(data);
		//client.write('Chat Server : Goodbye, ' + client.name + '. See you next time!\n');
		clientList.splice(clientList.indexOf(client),1);
		broadcast(client.name + "left the chat room.\n" , {name: "Chat Server"});
	});
	client.on('error', function(e){
		console.log(e);
	});



function broadcast(message, client){
	var removeClientList = [];
	clientList.forEach(function(participant){
		if(participant !== client){
			if(participant.writable){
				participant.write(client.name + ":" + message);
			} else {
				participant.destroy();
				removeClientList.push(participant);
			}
		}
	});
	removeClientList.forEach(function(bootedParticipant){
		clientList.splice(clientList.indexOf(bootedParticipant), 1);
	});
}
});

server.listen(9000);
server.on("load", function() { 
	console.log("Chat server loaded."); 
});
server.emit("load"); //custom event


