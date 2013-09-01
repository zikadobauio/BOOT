API.on(API.CHAT,onChat);
API.on(API.DJ_ADVANCE,onDjAdv);
API.sendChat('GM ORION CMD :D!');
var song;
if (API.getDJs().length == 0) API.on(API.DJ_UPDATE,djUpdate);
else song = API.getMedia().author + ' - ' + API.getMedia().title;

function onChat(data) {
	if (API.hasPermission(data.fromID,API.ROLE.BOUNCER)) {
		var message = data.message.toLowerCase();
		if (message.indexOf('!lock') == 0) API.moderateRoomProps(true,true);
		if (message.indexOf('!unlock') == 0) API.moderateRoomProps(false,true);
		if (message.indexOf('!lockskip') == 0) {
			API.moderateRoomProps(true,true);
			setTimeout(function(){API.moderateForceSkip();},1500);
			setTimeout(function(){API.moderateRoomProps(false,true);},4000);
		}
		if (message.indexOf('!skip') == 0) API.moderateForceSkip();
		if (message.indexOf('!ping') == 0) API.sendChat('Pong!');
		if (message.indexOf('!kill') == 0) {
			API.sendChat('bot shutting down.');
			API.off(API.CHAT,onChat);
			API.off(API.DJ_ADVANCE,onDjAdv);
		}
	}
}

function onDjAdv(obj) {
	setTimeout(function(){document.getElementById("button-vote-positive").click();},1500);
	var songStr = song;
	var woots = obj.lastPlay.score.positive, mehs = obj.lastPlay.score.negative, curates = obj.lastPlay.score.curates ;
	if (woots === 1) var ww = ' woot :+1:  '; else var ww = ' woots :-1:  ';
	if (mehs === 1) var mm = ' meh ~ '; else var mm = ' mehs :floppy_disk:  ';
	if (curates === 1) var cc = ' curate'; else var cc = ' curates';
	var scoreStr = ' :+1:  ' + woots + ww + mehs + mm + curates + cc;
	API.sendChat('/em: ' + songStr + scoreStr);
	song = API.getMedia().author + ' - ' + API.getMedia().title;
}

function djUpdate(djs) {
	song = API.getMedia().author + ' - ' + API.getMedia().title;
	API.off(API.DJ_UPDATE,djUpdate);
}

