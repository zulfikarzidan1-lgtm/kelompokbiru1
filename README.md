# kelompokbiru1

<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Kuis IPA - Kelompok Biru</title>
<style>
 body { font-family: Arial, sans-serif; background: #e7f3ff; padding: 20px; }
 .container { max-width: 700px; margin: auto; background: white; padding: 20px; border-radius: 10px; }
 h1 { text-align: center; color: #0059b3; }
 .question { margin-bottom: 20px; }
 button { padding: 10px 20px; margin: 10px 5px; cursor: pointer; border-radius: 5px; border: none; background: #0059b3; color: white; }
 #result { font-size: 20px; font-weight: bold; margin-top: 20px; }
 .nav-buttons { text-align: center; }
</style>
</head>
<body>
<div class="container">
<h1>Kuis IPA - Kelompok Biru</h1>
<p><strong>Anggota Kelompok:</strong><br>
1. Muhammad Wildan <br>
2. Selia Anggraeni <br>
3. Sarah Ristian <br>
4. Fajar Rizkiansyah <br>
5. Sigit Ardiansyah </p>

<div id="quiz"></div>
<div class="nav-buttons">
 <button id="prevBtn" onclick="prevPage()" style="display: none;">Sebelumnya</button>
 <button id="nextBtn" onclick="nextPage()">Selanjutnya</button>
 <button id="submitBtn" onclick="submitQuiz()" style="display: none;">Kumpulkan Jawaban</button>
</div>
<p id="result"></p>
</div>

<script>
const questions = [
 {q: "1. Apa planet terbesar di tata surya?", options: ["Bumi", "Jupiter", "Mars", "Venus"], answer: 1},
 {q: "2. Air menguap menjadi?", options: ["Cairan", "Padatan", "Gas", "Awan"], answer: 2},
 {q: "3. Hewan yang berkembang biak dengan bertelur disebut?", options: ["Vivipar", "Ovipar", "Ovovivipar", "Herbivora"], answer: 1},
 {q: "4. Sumber energi terbesar di bumi adalah?", options: ["Minyak bumi", "Matahari", "Air", "Angin"], answer: 1},
 {q: "5. Bagian tumbuhan yang berfungsi menyerap air?", options: ["Batang", "Akar", "Daun", "Bunga"], answer: 1},
 {q: "6. Proses fotosintesis terjadi pada bagian?", options: ["Daun", "Akar", "Biji", "Batang"], answer: 0},
 {q: "7. Zat yang dibutuhkan tumbuhan untuk fotosintesis adalah?", options: ["Karbon dioksida", "Oksigen", "Nitrogen", "Hidrogen"], answer: 0},
 {q: "8. Hewan pemakan tumbuhan disebut?", options: ["Omnivora", "Karnivora", "Herbivora", "Insektivora"], answer: 2},
 {q: "9. Bagian mata yang berfungsi mengatur banyak sedikitnya cahaya?", options: ["Kornea", "Lensa", "Pupil", "Retina"], answer: 2},
 {q: "10. Gas yang dihirup manusia untuk bernapas adalah?", options: ["Karbon dioksida", "Oksigen", "Nitrogen", "Helium"], answer: 1},
 {q: "11. Proses ketika air dari tumbuhan keluar melalui daun disebut?", options: ["Transpirasi", "Evaporasi", "Kondensasi", "Presipitasi"], answer: 0},
 {q: "12. Satuan untuk mengukur massa adalah?", options: ["Liter", "Meter", "Kilogram", "Derajat"], answer: 2},
 {q: "13. Gaya yang membuat benda jatuh ke bumi disebut?", options: ["Gaya gesek", "Gaya magnet", "Gaya gravitasi", "Gaya sentrifugal"], answer: 2},
 {q: "14. Zat yang menyebabkan zat lain larut disebut?", options: ["Pelarut", "Larutan", "Padatan", "Gas"], answer: 0},
 {q: "15. Energi yang dihasilkan dari reaksi pembakaran berasal dari?", options: ["Kalori", "Massa", "Ikatan kimia", "Suhu"], answer: 2},
 {q: "16. Alat untuk melihat benda sangat kecil adalah?", options: ["Mikroskop", "Teleskop", "Kamera", "Lensa"], answer: 0},
 {q: "17. Unsur yang membentuk udara yang kita hirup paling banyak adalah?", options: ["Oksigen", "Karbon dioksida", "Nitrogen", "Hidrogen"], answer: 2},
 {q: "18. Perubahan wujud dari gas langsung menjadi padat disebut?", options: ["Mencair", "Menyublim", "Kondensasi", "Deposis"], answer: 3},
 {q: "19. Bagian tumbuhan yang biasanya menyimpan cadangan makanan adalah?", options: ["Akar", "Bunga", "Daun", "Batang"], answer: 0},
 {q: "20. Sumber utama energi pada bumi yang memungkinkan fotosintesis adalah?", options: ["Matahari", "Bulan", "Batu bara", "Angin"], answer: 0}
];

let currentPage = 1;
const questionsPerPage = 10;
let userAnswers = new Array(questions.length).fill(null); // Array untuk menyimpan jawaban pengguna
let quizSubmitted = false; // Flag untuk menandai apakah kuis sudah disubmit

function loadQuiz(){
 let quizDiv = document.getElementById('quiz');
 quizDiv.innerHTML = "";
 if(quizSubmitted){
  // Tampilkan semua pertanyaan dengan hasil setelah submit
  questions.forEach((q,i)=>{
   let checkedValue = userAnswers[i];
   quizDiv.innerHTML += `<div class='question'>${q.q}<br>` + q.options.map((opt,j)=>{
    let color = "black";
    if(j === q.answer){
     color = "green"; // Jawaban benar
    }
    if(checkedValue !== null && checkedValue == j && checkedValue !== q.answer){
     color = "red"; // Jawaban salah yang dipilih
    }
    return `<label><input type='radio' name='q${i}' value='${j}' ${checkedValue == j ? 'checked' : ''} disabled><span class='option-text' style='color: ${color};'> ${opt}</span></label><br>`;
   }).join("") + `</div>`;
  });
 } else {
  // Tampilkan pertanyaan per halaman sebelum submit
  let start = (currentPage - 1) * questionsPerPage;
  let end = start + questionsPerPage;
  let pageQuestions = questions.slice(start, end);
  pageQuestions.forEach((q,i)=>{
   let globalIndex = start + i;
   let checkedValue = userAnswers[globalIndex];
   quizDiv.innerHTML += `<div class='question'>${q.q}<br>` + q.options.map((opt,j)=>`<label><input type='radio' name='q${globalIndex}' value='${j}' ${checkedValue == j ? 'checked' : ''} onchange='updateAnswer(${globalIndex}, ${j})'><span class='option-text'> ${opt}</span></label><br>`).join("") + `</div>`;
  });
 }
 updateButtons();
}

function updateAnswer(index, value){
 userAnswers[index] = value;
}

function updateButtons(){
 if(quizSubmitted){
  document.getElementById('prevBtn').style.display = 'none';
  document.getElementById('nextBtn').style.display = 'none';
  document.getElementById('submitBtn').style.display = 'none';
 } else {
  document.getElementById('prevBtn').style.display = currentPage > 1 ? 'inline-block' : 'none';
  document.getElementById('nextBtn').style.display = currentPage < 2 ? 'inline-block' : 'none';
  document.getElementById('submitBtn').style.display = currentPage === 2 ? 'inline-block' : 'none';
 }
}

function nextPage(){
 if(currentPage < 2 && !quizSubmitted){
  currentPage++;
  loadQuiz();
 }
}

function prevPage(){
 if(currentPage > 1 && !quizSubmitted){
  currentPage--;
  loadQuiz();
 }
}

function submitQuiz(){
 let score = 0;
 questions.forEach((q,i)=>{
  if(userAnswers[i] !== null && userAnswers[i] === q.answer){ score++; }
 });
 document.getElementById('result').innerText = `Skor Kamu: ${score} dari ${questions.length}`;
 quizSubmitted = true;
 loadQuiz(); // Muat ulang untuk menampilkan semua hasil dengan warna
}

loadQuiz();
</script>
</body>
</html>
