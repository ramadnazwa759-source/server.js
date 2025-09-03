CRUD UNTUK /movies dan /directors.

const express = require('express');
const app = express();
const port = 3100;

app.use(express.json());

let movies = [
    { id: 1, title: 'Inception', director: 'Christopher Nolan', year: 2010 },
    { id: 2, title: 'The Matrix', director: 'The Wachowskis', year: 1999 },
    { id: 3, title: 'Interstellar', director: 'Christopher Nolan', year: 2014 }
];

app.get('/', (req, res) => {
    res.send('Selamat datang di belajar API Dasar dengan express js')
});

app.get('/movies', (req, res) => {
    res.json(movies);
});

app.get('/movies/:id',(req, res)=>{
    const id=Number(req.params.id);
    const movie=movies.find(m=>m.id===id);
    if(!movie)returnres.status(404).json({error:'Movietidak →ditemukan'});
    res.json(movie);
});

app.get('/movies/title/:title', (req, res) => {
    const name = req.params.title.toLowerCase();
    const movie = movies.find(m => m.title.toLowerCase() === name);
    if (movie) {
        res.json(movie);
    } else {
        res.status(404).send('Movie not found');
    }
});

// POST /movies - Membuat film baru
idSeq = 4;
app.post('/movies', (req, res) => {
    const { title, director, year } = req.body || {};
    if (!title || !director || !year) {
     return res.status(400).json({ error: 'title, director, year,→ wajib diisi' });
    }
    const newMovie = { id: idSeq++, title, director, year };
    movies.push(newMovie);
    res.status(201).json(newMovie);
});

// PUT /movies/:id - Memperbarui data film
app.put('/movies/:id', (req, res) => {
    const id = Number(req.params.id);
    const movieIndex = movies.findIndex(m => m.id === id);
    if (movieIndex === -1) {
    return res.status(404).json({ error: 'Movie tidak ditemukan' });
 }
    const { title, director, year } = req.body || {};
    const updatedMovie = { id, title, director, year };
    movies[movieIndex] = updatedMovie;
    res.json(updatedMovie);
 });

 // Menghapus film
app.delete('/movies/:id', (req, res) => {
    const id = Number(req.params.id);
    const movieIndex = movies.findIndex(m => m.id === id);
    if (movieIndex === -1) {
    return res.status(404).json({ error: 'Movie tidak ditemukan',});
}
    movies.splice(movieIndex, 1);
    res.status(204).send();
});


// CRUD directors atau Sutradara

let directors = [
    { id: 1, name : 'Christopher Nolan', birthYear : 1900 },
    { id: 2, name : 'The Wachowskis'   , birthYear : 1840 },
    { id: 3, name : 'Rahmadan', birthYear : 1945 }
];

// Mengembalikan daftar semua sutradara.
app.get('/directors', (req, res) => {
    res.json(directors);
});

// Detail satu sutradara
app.get('/directors/:id',(req, res)=>{
    const id=Number(req.params.id);
    const director=directors.find(m=>m.id===id);
    if(!director)returnres.status(404).json({error:'directortidak →ditemukan'});
    res.json(director);
});

// Membuat seorang sutradara baru
idSeq = 4;
app.post('/directors', (req, res) => {
    const { name, birthYear } = req.body || {};
    if ( !name || !birthYear) {
     return res.status(400).json({ error: 'name , birthyear,→ wajib diisi' });
    }
    const newDirectors = { id: idSeq++, name, birthYear };
    directors.push(newDirectors);
    res.status(201).json(newDirectors);
});

// Memperbarui data sutradara
app.put('/directors/:id', (req, res) => {
  const id = Number(req.params.id);
  const directorsIndex = directors.findIndex(d => d.id === id);
  if (directorsIndex === -1) {
     return res.status(404).json({ error: 'Data tidak ditemukan,→' });
     }
     const { name, birthYear } = req.body || {};
     const updatedDirectors = { id, name, birthYear };
     directors[directorsIndex] = updatedDirectors;
     res.json(updatedDirectors);
  });

  // untuk menghapus data sutradara
  app.delete('/directors/:id', (req, res) => {
  const id = Number(req.params.id);
  const directorsIndex = directors.findIndex(d => d.id === id);
  if (directorsIndex === -1) {
     return res.status(404).json({ error: 'Data tidak ditemukan'});
     }
     directors.splice(directorsIndex, 1);
     res.status(204).send();
});

app.listen(port, () => {
    console.log(`Server berjalan di http://localhost:${port}`);
});


