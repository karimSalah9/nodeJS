# nodeJS
node js projects 

a project that handle hhtp requests (POST , GET , PUT , DELETE).

you must install some package:
1- npm i express
2- npm i joi

-simple card object with {id , name , email , subject}
perform CRUD opertion (create , update , delete , get  , get by id)

-validating user input using joi pacakage 
-parse body of request to json

for any support contact me : karim.salah.kotb@gmail.com



Code :




const express = require('express');

const app = express(); // for restful services

const Joi = require('joi'); // validator package

app.use(express.json()); // to parse body
//Create Card Object

var cards = [
    { id: 1, name: "karim", email: "key@gmail.com", subject: "not yet" },
    { id: 2, name: "ko", email: "ko@gmail.com", subject: "not you" },
    { id: 3, name: "koko", email: "koko@gmail.com", subject: "not me" },
    { id: 4, name: "kimo", email: "kimo@gmail.com", subject: "not him" },
    { id: 5, name: "kotb", email: "kotb@gmail.com", subject: "not yours" },
];
//Test card object
//console.log(card)

// Http get request (get all data in the object)

app.get('/api/cards', (req, res) => {
    res.send(cards);
});


// Http get request (get object  data by ID )

app.get('/api/cards/:id', (req, res) => {
    const card = cards.find(c => c.id === parseInt(req.params.id));
    if (!card) { res.status(404).send('no cards found with this id!'); };
    res.send(card);

});

//Http post request to add a card to object 

app.post('/api/cards/create', (req, res) => {
    // create schema for required inputs to validate 
    const schema = {

        name: Joi.string().alphanum().min(3).max(30).required(),
        email: Joi.string().email({ minDomainAtoms: 2 }),
        subject: Joi.string().min(15).required()
    };
    
    //validate user inputs with Joi
    
    const result = Joi.validate(req.body, schema);
    if (result.error) { return res.status(400).send(result.error.details[0].message); }
    // res.send(result);
    const card = {
        id: cards.length + 1,
        name: req.body.name,
        email: req.body.email,
        subject: req.body.subject
    };
    res.send(card);

});

//Http request to update selected card PUT method

app.put('/api/cards/:id', (req, res) => {
    const card = cards.find(c => c.id === parseInt(req.params.id));
    if (!card) { return res.status(400).send('this card with this id not found'); }
    const schema = {

        name: Joi.string().alphanum().min(3).max(30).required(),
        email: Joi.string().email({ minDomainAtoms: 2 }),
        subject: Joi.string().min(15).required()
    };
    var result = Joi.validate(req.body, schema);
    if (result.error) { return res.status(400).send(result.error.details[0].message); }
    // res.send(result);
    const index = cards.indexOf(card);
    const mycard = {
        id: parseInt(req.params.id),
        name: req.body.name,
        email: req.body.email,
        subject: req.body.subject
    };
    cards[index] = mycard;
    res.send(mycard);

});


//Delete Http Request by id

app.delete("/api/cards/:id", (req, res) => {
    const card = cards.find(c => c.id === parseInt(req.params.id));
    if (!card) { return res.status(400).send('No Card Found With This ID!!'); }
    const index = cards.indexOf(card);
    cards.splice(index, 1);
    res.send(cards)
});






//listen on port 3000
app.listen(3000);
console.log("starting on port 3000...")


