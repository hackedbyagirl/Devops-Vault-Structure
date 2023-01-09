<%*
let title = tp.file.title;
var note;
////////////////////////////////////// Main //////////////////////////////////////

// Present selection of Search Tags to user
let tag = await tp.system.suggester(["Primary", "Secondary", "Project Overview", "Project Issue", "User Story", "Basic", "Tool", "README", "Personal"], ["Primary", "Secondary", "Project Overview", "Project Issue", "User Story", "Basic", "Tool", "README", "Personal"], true)

await setNoteStructure(tag, title);

////////////////////////////////// Functions /////////////////////////////////////
async function setNoteStructure(note_tag, noteTitle) {
	console.log("Getting Struct MD for tag: " + note_tag);

	var struct;
	var file_destination;
	var isCat = true;

	if (note_tag.startsWith("Primary")) {
		struct = "[[0401 - Primary Category]]";
		file_destination = "01 - Primary Categories/";
	} 
	else if (note_tag.startsWith("Secondary")) {
		struct = "[[0402 - Secondary Category]]";
		file_destination = "02 - Secondary Categories/";
	} 
	else if (note_tag.startsWith("Project Overview")) {
		struct = "[[0405 - Project Overview]]";
		file_destination = "03 - Content/";
		isCat = false;
	} 
	else if (note_tag.startsWith("Project Issue")) {
		struct = "[[0406 - Project Issue]]";
		file_destination = "03 - Content/";
		isCat = false;
	}
	else if (note_tag.startsWith("User Story")) {
		struct = "[[0407 - User Story]]";
		file_destination = "03 - Content/";
		isCat = false;
	}
	else if (note_tag.startsWith("Basic")) {
		struct = "[[0408 - Basic]]";
		file_destination = "03 - Content/";
		isCat = false;
	}
	else if (note_tag.startsWith("Tool")) {
		struct = "[[0409 - Tool]]";
		file_destination = "03 - Content/";
		isCat = false;
	}	 	 	 
	else if (note_tag.startsWith("README")) {
		struct = "[[0410 - README]]";
		file_destination = "03 - Content/";
		isCat = false;
	}
	else if (note_tag.startsWith("Personal")) {
		struct = "[[0411 - Personal]]";
		file_destination = "05 - Personal/";
		isCat = false;
	}	 
	else {
		console.log("You selected an option outside what was expected.");
		console.log("Try again.");	
	}
	await buildNote(noteTitle, struct, file_destination, isCat);
}

async function buildNote(notetitle, content, dest, category){
	let metadata = "[[0404 - Metadata]]";
	let header = "[[0403 - Content Header]]";
	let resources = "[[0412 - Resources]]";
	
	var meta = await tp.file.include(metadata);
	var body = await tp.file.include(content);
	var head = await tp.file.include(header);
	var body = await tp.file.include(content);
	var resc = await tp.file.include(resources);
	
	if (category === true) {
		await tp.file.move(dest + notetitle);		
		note = meta + body;
	}
	else if (category === false) {
		await tp.file.move(dest + notetitle);
		note = meta + head + body + resc;
	}
}
%>
<%* tR += `${note}` %>
