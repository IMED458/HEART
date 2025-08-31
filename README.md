// React-ის ძირითადი ბიბლიოთეკის იმპორტი
import { useState } from "react";

// UI კომპონენტების იმპორტი shadcn/ui-დან (Card, CardContent, Button)
// ეს კომპონენტები უზრუნველყოფენ ვიზუალურ სტილს და ფუნქციონალურობას
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";

// კითხვის ტექსტი, რომელიც გამოჩნდება თითოეული აუდიოსთვის
const questionText = "ამოიცანით გულის ტონი";

// პასუხის ვარიანტები, რომლებიც გამოჩნდება თითოეული კითხვის ქვეშ
const answerOptions = [
  "გულის მესამე ტონი",
  "გულის მე-4 ტონი",
  "პირველი ტონის გახლეჩა",
  "მეორე ტონის გახლეჩა-გაორება",
  "პერიკარდიუმის ხახუნის ხმიანობა",
  "ტკაცუნა პირველი ტონი",
  "მეორე ტონის შესუსტება და უხეში სისტოლური შუილი აორტაზე",
  "მეორე ტონის შესუსტება და დეკრეშჩენდოს ტიპის დიასტოლური შუილი აორტაზე",
  "კრეშჩენდო-დეკრეშჩენდო ტიპის სისტოლური შუილი, მეორე ტონის გაორება",
  "ფუნქციური სისტოლური შუილი",
  "ჰოლოსისტოლური შუილი"
];

// კითხვების მასივი, რომელიც აერთიანებს აუდიო ფაილებს და სწორ პასუხებს
// `audio` გზები ახლა მიუთითებს Google Drive-ის პირდაპირ გადმოწერის ბმულებზე.
// დარწმუნდით, რომ თითოეული ფაილის წვდომა დაყენებულია "Anyone with the link"-ზე Google Drive-ზე.
const questions = [
  { id: 1, audio: "https://drive.google.com/uc?export=download&id=1RMyVF96Gif-A6H5gHkTbaEJmSXsiqUQI", correct: 0 },
  { id: 2, audio: "https://drive.google.com/uc?export=download&id=1gZPbTOgXRuWj95CBzLm_3ZZwbr6rpe2j", correct: 1 },
  { id: 3, audio: "https://drive.google.com/uc?export=download&id=16qV5ySIdba0V8k_n0T7JB2Tf8wWzFxW3", correct: 2 },
  { id: 4, audio: "https://drive.google.com/uc?export=download&id=1-YAnkTfyK6uv28P3FG10lmzp2mQNZjRu", correct: 3 },
  { id: 5, audio: "https://drive.google.com/uc?export=download&id=1rqQD_g9dEicE1RZwuQg8lnLgEG8I7LDa", correct: 4 },
  { id: 6, audio: "https://drive.google.com/uc?export=download&id=1ZyVxeV_ZIN1h3RWcq20uWpz2Mu6iQakn", correct: 5 },
  { id: 7, audio: "https://drive.google.com/uc?export=download&id=1-xJeCFlM8GukAxm1GxTwu_5pjQRsSODr", correct: 6 },
  { id: 8, audio: "https://drive.google.com/uc?export=download&id=1o4F2GL6-djigT2DYpdGsyKWuZTeDEXVT", correct: 7 },
  { id: 9, audio: "https://drive.google.com/uc?export=download&id=1RQaEjjg2fEVgNV_zkQ9eHftOzgA0Q0rh", correct: 8 },
  { id: 10, audio: "https://drive.google.com/uc?export=download&id=1y4UyCiLlZnV-KItEiKU7DQrFkN2QNvNG", correct: 9 },
  { id: 11, audio: "https://drive.google.com/uc?export=download&id=1kIJlUjIqNMI3Qqw49pHfaFn0Q5GAcNMt", correct: 10 },
];


// მთავარი აპლიკაციის კომპონენტი
export default function App() {
  // `answers` მდგომარეობა ინახავს მომხმარებლის მიერ არჩეულ პასუხებს
  // ობიექტი, სადაც გასაღები არის კითხვის ID, ხოლო მნიშვნელობა - არჩეული პასუხის ინდექსი
  const [answers, setAnswers] = useState({});
  // `score` მდგომარეობა ინახავს მომხმარებლის საბოლოო ქულას
  const [score, setScore] = useState(null);

  // ფუნქცია, რომელიც ამუშავებს მომხმარებლის მიერ პასუხის არჩევას
  const handleSelect = (qid, index) => {
    setAnswers((prev) => ({ ...prev, [qid]: index }));
  };

  // ფუნქცია, რომელიც ითვლის მომხმარებლის ქულას
  const calculateScore = () => {
    let sc = 0; // ქულის საწყისი მნიშვნელობა
    // თითოეულ კითხვაზე გავლისას ვამოწმებთ, ემთხვევა თუ არა მომხმარებლის პასუხი სწორ პასუხს
    questions.forEach((q) => {
      if (answers[q.id] === q.correct) sc++; // თუ ემთხვევა, ქულა იზრდება
    });
    setScore(sc); // ქულის განახლება
  };

  return (
    // მთავარი კონტეინერი Tailwind CSS კლასებით ცენტრირებისთვის და დაშორებისთვის
    <div className="max-w-4xl mx-auto p-4 space-y-6 font-inter">
      {/* სათაური */}
      <h1 className="text-2xl font-bold text-gray-800 text-center rounded-md p-2 bg-blue-100 shadow-md">აუსკულტაციის ტესტი</h1>

      {/* კითხვების რენდერირება */}
      {questions.map((q) => (
        <Card key={q.id} className="bg-white shadow-lg rounded-lg p-4 border border-blue-200">
          <CardContent className="space-y-3">
            {/* კითხვის ტექსტი */}
            <p className="font-semibold text-gray-700">{questionText}</p>
            {/* აუდიო პლეერი */}
            <audio controls src={q.audio} className="w-full rounded-md shadow-sm" />
            {/* პასუხის ვარიანტები */}
            <div className="space-y-2">
              {answerOptions.map((c, i) => (
                <div key={i} className="flex items-center gap-2 bg-blue-50 p-2 rounded-md hover:bg-blue-100 transition-colors duration-200">
                  <input
                    type="radio"
                    name={`q-${q.id}`} // უნიკალური სახელი თითოეული კითხვისთვის
                    checked={answers[q.id] === i} // შემოწმება, არის თუ არა ეს ვარიანტი არჩეული
                    onChange={() => handleSelect(q.id, i)} // არჩევისას მდგომარეობის განახლება
                    className="form-radio h-4 w-4 text-blue-600 transition-colors duration-200"
                  />
                  <label className="text-gray-700 cursor-pointer">{c}</label>
                </div>
              ))}
            </div>
          </CardContent>
        </Card>
      ))}

      {/* ქულის გამოთვლის ღილაკი */}
      <Button
        onClick={calculateScore}
        className="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-md shadow-md transition-all duration-300 transform hover:scale-105"
      >
        ქულის გამოთვლა
      </Button>

      {/* ქულის ჩვენება, თუ გამოთვლილია */}
      {score !== null && (
        <p className="text-lg font-semibold text-center text-gray-800 bg-green-100 p-3 rounded-md shadow-md">
          შენი ქულაა: {score} / {questions.length}
        </p>
      )}
    </div>
  );
}
