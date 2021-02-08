# note-taker
 This simple note taking application allows you to save notes with a title and plain text. Powered by ExpressJS, the Note Taker app makes creating, viewing, and deleting notes a cinch

Prerequisites

https://nodejs.org/

Run npm install to install all dependencies. To use the application locally, run node server.js in your CLI, and then open http://localhost:3000 in your preferred browswer.

Deployed Link:

** CODE SNIPPETS **

The following code snippet shows how the Express server initialization and setup. Routes are setup in a separate file for organization purposes...

// Initialize express app
const app = express();
const PORT = process.env.PORT || 3000;

// Setup data parsing
app.use(express.urlencoded({ extended: true }));
app.use(express.json());
app.use(express.static(__dirname));

//Require routes file
require('./routes/routes')(app);

// Setup listener
app.listen(PORT, function() {
    console.log("App listening on PORT: " + PORT);
});  




The following code snippet shows how this app sets up Express routing and uses a JSON file for CRUD interactions.




module.exports = app => {

    // Setup notes variable
    fs.readFile("db/db.json","utf8", (err, data) => {

        if (err) throw err;

        // Store the contents of the db.json file in a variable for better performance
        var notes = JSON.parse(data);

        // API ROUTES
        // ========================================================
    
        // Setup the /api/notes get route
        app.get("/api/notes", function(req, res) {
            // Read the db.json file and return all saved notes as JSON.
            res.json(notes);
        });

        // Setup the /api/notes post route
        app.post("/api/notes", function(req, res) {
            // Receives a new note, adds it to db.json, then returns the new note
            let newNote = req.body;
            notes.push(newNote);
            updateDb();
            return console.log("Added new note: "+newNote.title);
        });

        // Retrieves a note with specific id
        app.get("/api/notes/:id", function(req,res) {
            // display json for the notes array indices of the provided id
            res.json(notes[req.params.id]);
        });

        // Deletes a note with specific id
        app.delete("/api/notes/:id", function(req, res) {
            notes.splice(req.params.id, 1);
            updateDb();
            console.log("Deleted note with id "+req.params.id);
        });

        // VIEW ROUTES
        // ========================================================

        // Display notes.html when /notes is accessed
        app.get('/notes', function(req,res) {
            res.sendFile(path.join(__dirname, "../public/notes.html"));
        });
        
        // Display index.html when all other routes are accessed
        app.get('*', function(req,res) {
            res.sendFile(path.join(__dirname, "../public/index.html"));
        });

        //updates the json file whenever a note is added or deleted
        function updateDb() {
            fs.writeFile("db/db.json",JSON.stringify(notes,'\t'),err => {
                if (err) throw err;
                return true;
            });
        }

    });

}




 BUILT WITH 

 - JavaScript
 - NodeJS
 - Node Packages
 -  - Express 
