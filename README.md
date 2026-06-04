import React, { useState, useEffect } from 'react';
import { 
  Home, BookOpen, PenTool, MessageCircle, User, Award, 
  Settings, CheckCircle, Heart, Bookmark, LogOut,
  Search, Play, Star, Flame, Trophy, Moon, Sun, Users, BarChart, X
} from 'lucide-react';

// ==========================================
// 1. DATA & CONTENT (FULL CURRICULUM: 0 TO ADVANCED)
// ==========================================

const VOCABULARY_DB = [
  { id: 1, word: "Cat", ipa: "/kæt/", pos: "Noun", meaning: "แมว", synonyms: ["Feline", "Kitty"], exampleEn: "I have a cute cat.", exampleTh: "ฉันมีแมวที่น่ารักหนึ่งตัว", difficulty: "Beginner", category: "Animals" },
  { id: 2, word: "Dog", ipa: "/dɔːɡ/", pos: "Noun", meaning: "หมา, สุนัข", synonyms: ["Hound", "Pup"], exampleEn: "The dog is running fast.", exampleTh: "หมากำลังวิ่งอย่างรวดเร็ว", difficulty: "Beginner", category: "Animals" },
  { id: 3, word: "Hello", ipa: "/həˈloʊ/", pos: "Greeting", meaning: "สวัสดี", synonyms: ["Hi", "Greetings"], exampleEn: "Hello, how are you?", exampleTh: "สวัสดี คุณสบายดีไหม?", difficulty: "Beginner", category: "General" },
  { id: 4, word: "Water", ipa: "/ˈwɔːtər/", pos: "Noun", meaning: "น้ำ", synonyms: ["Aqua"], exampleEn: "I drink water every day.", exampleTh: "ฉันดื่มน้ำทุกวัน", difficulty: "Beginner", category: "Food" },
  { id: 5, word: "Commute", ipa: "/kəˈmjuːt/", pos: "Verb", meaning: "เดินทางไปกลับระหว่างบ้านและที่ทำงาน", synonyms: ["Travel"], exampleEn: "It takes an hour to commute.", exampleTh: "ใช้เวลาหนึ่งชั่วโมงในการเดินทางไปทำงาน", difficulty: "Intermediate", category: "Daily Life" },
  { id: 6, word: "Resilient", ipa: "/rɪˈzɪliənt/", pos: "Adjective", meaning: "ฟื้นตัวหรือปรับตัวจากสถานการณ์ยากลำบากได้ดี", synonyms: ["Strong", "Adaptable", "Tough"], exampleEn: "Nurses must be resilient.", exampleTh: "พยาบาลต้องเข้มแข็งอดทน", difficulty: "Advanced", category: "Work" },
  { id: 7, word: "Procrastinate", ipa: "/proʊˈkræstɪneɪt/", pos: "Verb", meaning: "ผัดวันประกันพรุ่ง", synonyms: ["Delay", "Postpone"], exampleEn: "Stop procrastinating and do your homework.", exampleTh: "เลิกผัดวันประกันพรุ่งแล้วทำการบ้านได้แล้ว", difficulty: "Intermediate", category: "School" },
  { id: 8, word: "Meticulous", ipa: "/məˈtɪkjələs/", pos: "Adjective", meaning: "พิถีพิถัน, ระมัดระวังในรายละเอียด", synonyms: ["Careful", "Thorough"], exampleEn: "He is meticulous about his work.", exampleTh: "เขาพิถีพิถันกับงานของเขามาก", difficulty: "Advanced", category: "Work" }
];

const GRAMMAR_MODULES = [
  {
    id: "m1", title: "บทที่ 1: พื้นฐานตัวอักษรและประธาน (เริ่มจาก 0)",
    lessons: [
      {
        id: "m1-l1", title: "1.1 สระ พยัญชนะ และตัวเลข",
        explanation: "ภาษาอังกฤษเริ่มต้นจากการรู้จักตัวอักษร A-Z (มี 26 ตัว) แบ่งเป็นสระ 5 ตัว (A, E, I, O, U) และพยัญชนะ 21 ตัว\n\nส่วนตัวเลขพื้นฐานคือ: 1 (One), 2 (Two), 3 (Three), 4 (Four), 5 (Five), 6 (Six), 7 (Seven), 8 (Eight), 9 (Nine), 10 (Ten)",
        structure: "สระ (Vowels) = A, E, I, O, U",
        examples: [
          { en: "A is for Apple", th: "A ย่อมาจากแอปเปิ้ล" },
          { en: "I have two dogs.", th: "ฉันมีหมา 2 ตัว (Two = 2)" }
        ],
        commonMistakes: "ประโยคภาษาอังกฤษต้องขึ้นต้นด้วยตัวพิมพ์ใหญ่ (Capital Letter) เสมอ!",
        summary: "จำสระ A E I O U ให้แม่น เพราะต้องใช้คู่กับ a, an",
        quiz: [
          { q: "สระในภาษาอังกฤษมี 5 ตัว คืออะไรบ้าง?", options: ["A, B, C, D, E", "A, E, I, O, U", "X, Y, Z, W, V"], ans: 1, exp: "สระมี 5 ตัวคือ A, E, I, O, U" },
          { q: "เลข 3 ในภาษาอังกฤษเขียนอย่างไร?", options: ["Tree", "Three", "Tea"], ans: 1, exp: "3 คือ Three" },
          { q: "ข้อใดคือตัวอักษรพิมพ์เล็กของ B ?", options: ["d", "p", "b"], ans: 2, exp: "B พิมพ์เล็กคือ b" }
        ]
      },
      {
        id: "m1-l2", title: "1.2 ประธานของประโยค (I, You, We...)",
        explanation: "ก่อนจะแต่งประโยคได้ เราต้องรู้จัก 'ประธาน' หรือผู้กระทำกันก่อน\n\n- I = ฉัน\n- You = คุณ\n- We = พวกเรา\n- They = พวกเขา/พวกมัน\n- He = เขาผู้ชาย\n- She = หล่อนผู้หญิง\n- It = มัน",
        structure: "เอกพจน์ (1 คน/สิ่ง) = I, He, She, It\nพหูพจน์ (หลายคน/สิ่ง) = You, We, They",
        examples: [
          { en: "She is my mother.", th: "หล่อนคือแม่ของฉัน" },
          { en: "They are dogs.", th: "พวกมันคือหมา (หลายตัว)" }
        ],
        commonMistakes: "ใช้ They แทนคนเดียว (ที่ไม่ใช่รูปพิเศษ) หรือใช้ It แทนคน",
        summary: "จำง่ายๆ: I (ฉัน), You (คุณ), We (พวกเรา), They (พวกเขา), He (เขาชาย), She (เขาหญิง), It (มัน)",
        quiz: [
          { q: "คำว่า 'พวกเรา' ตรงกับคำไหน?", options: ["They", "You", "We"], ans: 2, exp: "We = พวกเรา" },
          { q: "Lisa เป็นผู้หญิง 1 คน ใช้สรรพนามใดแทน?", options: ["He", "She", "It"], ans: 1, exp: "Lisa เป็นผู้หญิงใช้ She" },
          { q: "ถ้าเห็นหมา 3 ตัวกำลังวิ่ง จะใช้คำใดแทน?", options: ["It", "He", "They"], ans: 2, exp: "สัตว์หลายตัวใช้ They" }
        ]
      }
    ]
  },
  {
    id: "m2", title: "บทที่ 2: กริยาพื้นฐาน (Verb to be)",
    lessons: [
      {
        id: "m2-l1", title: "2.1 การใช้ Is, Am, Are",
        explanation: "Verb to be แปลว่า เป็น, อยู่, หรือ คือ เป็นหัวใจสำคัญของภาษาอังกฤษ กฎการใช้คู่กับประธานมีดังนี้:\n\n- I ใช้กับ am เสมอ (I am)\n- ประธาน 1 คน/สิ่ง (He, She, It, นามเอกพจน์) ใช้ is\n- ประธานหลายคน (You, We, They, นามพหูพจน์) ใช้ are",
        structure: "I + am\nHe, She, It + is\nYou, We, They + are",
        examples: [
          { en: "I am a student.", th: "ฉัน (เป็น) นักเรียน" },
          { en: "She is at home.", th: "หล่อน (อยู่) ที่บ้าน" },
          { en: "They are happy.", th: "พวกเรา (คือ/เป็นคน) มีความสุข" }
        ],
        commonMistakes: "ใช้สลับกัน เช่น I is, He are, You am (แบบนี้ผิด)",
        summary: "I คู่ am / ประธาน 1 คนคู่ is / ประธานหลายคนคู่ are",
        quiz: [
          { q: "I ต้องคู่กับอะไรเสมอ?", options: ["is", "am", "are"], ans: 1, exp: "I ต้องคู่กับ am" },
          { q: "They _______ my friends.", options: ["is", "am", "are"], ans: 2, exp: "They (หลายคน) ต้องใช้ are" },
          { q: "He _______ a doctor.", options: ["is", "am", "are"], ans: 0, exp: "He (คนเดียว) ต้องใช้ is" }
        ]
      }
    ]
  },
  {
    id: "m3", title: "บทที่ 3: คำนำหน้านาม และบุพบท",
    lessons: [
      {
        id: "m3-l1", title: "3.1 การใช้ A, An, The",
        explanation: "A/An ใช้กับนามนับได้เอกพจน์ที่ไม่ชี้เฉพาะ (A นำหน้าเสียงพยัญชนะ, An นำหน้าเสียงสระ A, E, I, O, U). The ใช้กับคำนามที่ชี้เฉพาะเจาะจง หรือพูดถึงเป็นครั้งที่สอง",
        structure: "a/an + คำนามทั่วไป 1 สิ่ง | the + คำนามชี้เฉพาะ",
        examples: [
          { en: "I saw a dog.", th: "ฉันเห็นหมาตัวหนึ่ง (ตัวไหนก็ไม่รู้)" },
          { en: "The dog bit me.", th: "หมาตัวนั้นแหละกัดฉัน (คนฟังรู้ว่าหมาตัวไหน)" },
          { en: "She eats an apple.", th: "หล่อนกินแอปเปิ้ล (an นำหน้าเสียงสระ a)" }
        ],
        commonMistakes: "ใช้ 'a' กับคำที่ขึ้นต้นด้วยเสียงสระ เช่น a apple (ผิด ต้อง an apple)",
        summary: "A/An = ทั่วไปหนึ่งสิ่ง, The = ชี้เฉพาะเจาะจง",
        quiz: [
          { q: "ฉันมีแมวหนึ่งตัว (ตัวไหนก็ได้): I have ___ cat.", options: ["a", "an", "the"], ans: 0, exp: "cat ไม่ได้ขึ้นต้นด้วยสระ และไม่ชี้เฉพาะ ใช้ a" },
          { q: "หล่อนมีร่มหนึ่งคัน: She has ___ umbrella.", options: ["a", "an", "the"], ans: 1, exp: "umbrella ขึ้นต้นด้วยเสียงสระ u ใช้ an" },
          { q: "พระอาทิตย์สว่างมาก: ___ sun is bright.", options: ["A", "An", "The"], ans: 2, exp: "พระอาทิตย์มีดวงเดียว เป็นสิ่งที่เฉพาะเจาะจง ต้องใช้ The" }
        ]
      },
      {
        id: "m3-l2", title: "3.2 การใช้ In, On, At",
        explanation: "In (กว้าง/ใหญ่/ข้างใน), On (บนพื้นผิว/วันในสัปดาห์), At (จุดที่เจาะจง/เวลาเป๊ะๆ)",
        structure: "In + เดือน/ปี/ประเทศ | On + วัน/ถนน | At + เวลา/สถานที่เจาะจง",
        examples: [
          { en: "I live in Thailand.", th: "ฉันอาศัยอยู่ในไทย" },
          { en: "See you on Monday.", th: "เจอกันวันจันทร์นะ" },
          { en: "Let's meet at 5 PM.", th: "เจอกันตอน 5 โมงเย็น" }
        ],
        commonMistakes: "แปลตรงตัวจากภาษาไทย เช่น 'In Monday' (ในวันจันทร์) ผิด ต้องใช้ 'On Monday'",
        summary: "In (เดือน/ปี/ประเทศ) > On (วัน) > At (เวลาเจาะจง)",
        quiz: [
          { q: "ฉันเกิดเดือนมกราคม: I was born ___ January.", options: ["in", "on", "at"], ans: 0, exp: "เดือน ใช้ in" },
          { q: "เจอกันวันศุกร์: See you ___ Friday.", options: ["in", "on", "at"], ans: 1, exp: "วันในสัปดาห์ ใช้ on" },
          { q: "ประชุมตอน 9 โมง: Meeting ___ 9:00 AM.", options: ["in", "on", "at"], ans: 2, exp: "เวลาเจาะจง ใช้ at" }
        ]
      }
    ]
  },
  {
    id: "m4", title: "บทที่ 4: กาลเวลา (Tenses เบื้องต้น)",
    lessons: [
      {
        id: "m4-l1", title: "4.1 Present Simple Tense",
        explanation: "เราใช้ Present Simple เพื่อพูดถึงสิ่งที่เป็นความจริงเสมอ, นิสัย, หรือกิจวัตรประจำวัน\n\nกฎสำคัญ: ถ้าประธานมีคนเดียว (He, She, It) กริยาต้องเติม s หรือ es",
        structure: "Subject + Verb 1 (เติม s/es ถัาประธานเอกพจน์)",
        examples: [
          { en: "I wake up at 7 AM.", th: "ฉันตื่นนอนตอน 7 โมงเช้า (I ไม่ต้องเติม s)" },
          { en: "He plays football every Sunday.", th: "เขาเล่นฟุตบอลทุกวันอาทิตย์ (He คนเดียว เติม s ที่ play)" }
        ],
        commonMistakes: "ลืมเติม s/es เช่น 'He play' (ผิด) ต้องเป็น 'He plays'",
        summary: "ใช้กับ ความจริง, กิจวัตร ประธานเอกพจน์กริยาเติม s/es",
        quiz: [
          { q: "He _______ to school every day.", options: ["go", "goes", "going"], ans: 1, exp: "ประธาน He คนเดียว กริยาต้องเติม s/es เป็น goes" },
          { q: "I _______ pizza.", options: ["like", "likes", "liking"], ans: 0, exp: "ประธาน I ไม่ต้องเติม s ที่กริยา ใช้ like ปกติ" },
          { q: "The sun _______ in the east.", options: ["rise", "rises", "rising"], ans: 1, exp: "พระอาทิตย์มีดวงเดียว กริยาเติม s เป็น rises" }
        ]
      },
      {
        id: "m4-l2", title: "4.2 Present Continuous",
        explanation: "ใช้พูดถึงสิ่งที่ 'กำลังเกิดขึ้น' ในขณะนี้ โครงสร้างคือ Verb to be + กริยาเติม ing",
        structure: "Subject + is/am/are + Verb(ing)",
        examples: [
          { en: "I am studying right now.", th: "ฉันกำลังเรียนอยู่ตอนนี้" },
          { en: "She is sleeping.", th: "หล่อนกำลังนอนหลับ" }
        ],
        commonMistakes: "ลืม is, am, are เช่น 'I studying' (ผิด) ต้องเป็น 'I am studying'",
        summary: "กำลังทำ = is/am/are + V.ing",
        quiz: [
          { q: "I _______ eating an apple now.", options: ["am", "is", "are"], ans: 0, exp: "I คู่กับ am เสมอ" },
          { q: "They are _______ football.", options: ["play", "plays", "playing"], ans: 2, exp: "หลัง Verb to be ใน Tense นี้ต้องเติม ing" },
          { q: "Look! The cat _______ running.", options: ["am", "is", "are"], ans: 1, exp: "The cat (แมวตัวเดียว) ใช้ is" }
        ]
      }
    ]
  },
  {
    id: "m5", title: "บทที่ 5: ไวยากรณ์ขั้นสูง (Advanced)",
    lessons: [
      {
        id: "m5-l1", title: "5.1 Passive Voice (ประโยคถูกกระทำ)",
        explanation: "Passive Voice เน้นไปที่ 'ผู้ถูกกระทำ' (Object) หรือ 'สิ่งที่เกิดขึ้น' มากกว่า 'ผู้กระทำ' (Subject) มักใช้ในงานเขียนเชิงการการหรือข่าว",
        structure: "Object + Verb to be + Verb 3 (Past Participle)",
        examples: [
          { en: "The book was written by John.", th: "หนังสือเล่มนี้ถูกเขียนโดยจอห์น" },
          { en: "My car is being repaired.", th: "รถของฉันกำลังถูกซ่อมอยู่" }
        ],
        commonMistakes: "ลืมเปลี่ยนกริยาหลักเป็น Verb 3",
        summary: "เน้นกรรม เอาขึ้นต้นประโยค ตามด้วย V.be + V.3 เสมอ",
        quiz: [
          { q: "The Eiffel Tower _______ built in 1889.", options: ["was", "is", "has"], ans: 0, exp: "เป็นเรื่องในอดีต (1889) ต้องใช้ Verb to be ช่อง 2 คือ was" },
          { q: "English _______ spoken all over the world.", options: ["is", "was", "are"], ans: 0, exp: "เป็นความจริงในปัจจุบัน ใช้ is (English ถือเป็นเอกพจน์)" },
          { q: "The window was _______ by the boy.", options: ["break", "broke", "broken"], ans: 2, exp: "หลัง Verb to be ใน Passive Voice ต้องเป็น V.3 (broken)" }
        ]
      }
    ]
  }
];

const CONVERSATIONS = [
  {
    id: "c1",
    title: "บทสนทนา: การทักทายง่ายๆ",
    category: "Greetings",
    dialogue: [
      { speaker: "John", en: "Hello! Good morning.", th: "สวัสดีตอนเช้า!" },
      { speaker: "Mary", en: "Hi John! How are you today?", th: "สวัสดีจอห์น! วันนี้คุณสบายดีไหม?" },
      { speaker: "John", en: "I am fine, thank you. And you?", th: "ฉันสบายดี ขอบคุณนะ แล้วคุณล่ะ?" },
      { speaker: "Mary", en: "I am doing great. Nice to meet you.", th: "ฉันสบายดีมาก ยินดีที่ได้รู้จักนะ" }
    ],
    vocab: ["Hello (สวัสดี)", "Good morning (สวัสดีตอนเช้า)", "How are you? (สบายดีไหม)", "Nice to meet you (ยินดีที่ได้รู้จัก)"]
  },
  {
    id: "c2",
    title: "บทสนทนา: สัมภาษณ์งาน",
    category: "Work",
    dialogue: [
      { speaker: "HR", en: "Can you tell me a little about yourself?", th: "ช่วยเล่าเรื่องเกี่ยวกับตัวคุณให้ฟังหน่อยได้ไหมครับ?" },
      { speaker: "You", en: "I recently graduated with a degree in Marketing. I am a fast learner.", th: "ฉันเพิ่งเรียนจบสาขาการตลาดค่ะ ฉันเป็นคนเรียนรู้เร็ว" },
      { speaker: "HR", en: "Why do you want to work for our company?", th: "ทำไมคุณถึงอยากทำงานกับบริษัทของเรา?" },
      { speaker: "You", en: "I admire your innovative products and company culture.", th: "ฉันชื่นชมผลิตภัณฑ์ที่ล้ำสมัยและวัฒนธรรมองค์กรของคุณค่ะ" }
    ],
    vocab: ["Graduated (เรียนจบ)", "Fast learner (คนเรียนรู้เร็ว)", "Innovative (ล้ำสมัย)", "Culture (วัฒนธรรมองค์กร)"]
  }
];

// ==========================================
// 2. STYLES
// ==========================================

const globalStyles = `
  @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=Noto+Sans+Thai:wght@400;500;600;700&family=Playfair+Display:wght@700&display=swap');

  :root {
    --c-seashell: #FFF4EB;
    --c-wheat: #F6E0B6;
    --c-powder-blue: #A6BCC9;
    --c-french-blue: #3E4B8E;
    --c-midnight: #3D1534;
  }

  .dark {
    --c-seashell: #1a1a2e;
    --c-wheat: #d4af37;
    --c-powder-blue: #16213e;
    --c-french-blue: #0f3460;
    --c-midnight: #e94560;
  }

  body {
    background-color: var(--c-seashell);
    color: var(--c-midnight);
    font-family: 'Noto Sans Thai', 'Inter', sans-serif;
    transition: background-color 0.3s ease, color 0.3s ease;
    margin: 0;
  }

  .font-heading { font-family: 'Playfair Display', serif; }
  .hide-scrollbar::-webkit-scrollbar { display: none; }
  .hide-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
  
  .academic-shadow { box-shadow: 0 10px 25px -5px rgba(62, 75, 142, 0.08); }
  .dark .academic-shadow { box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.5); }
`;

// ==========================================
// 3. COMPONENTS
// ==========================================

const Card = ({ children, className = "", onClick = null }) => (
  <div 
    onClick={onClick}
    className={`bg-white dark:bg-gray-800 rounded-[2rem] academic-shadow p-6 transition-all duration-300 ${onClick ? 'cursor-pointer hover:-translate-y-1 hover:shadow-xl' : ''} ${className}`}
  >
    {children}
  </div>
);

const ProgressBar = ({ progress, color = "bg-[#3E4B8E]", height = "h-3" }) => (
  <div className={`w-full bg-gray-200 dark:bg-gray-700 rounded-full ${height} overflow-hidden`}>
    <div className={`${height} rounded-full transition-all duration-1000 ease-out ${color}`} style={{ width: `${progress}%` }} />
  </div>
);

const Badge = ({ icon: Icon, title, active }) => (
  <div className={`flex flex-col items-center p-3 rounded-2xl border-2 transition-all ${active ? 'border-[#F6E0B6] bg-[#F6E0B6]/20' : 'border-gray-200 dark:border-gray-700 grayscale opacity-50'}`}>
    <div className={`p-3 rounded-full mb-2 ${active ? 'bg-[#F6E0B6] text-[#3D1534]' : 'bg-gray-200 text-gray-500'}`}>
      <Icon size={24} />
    </div>
    <span className="text-xs font-semibold text-center leading-tight">{title}</span>
  </div>
);

// ==========================================
// 4. MAIN APP
// ==========================================

export default function App() {
  const [currentView, setCurrentView] = useState('home');
  const [darkMode, setDarkMode] = useState(false);
  const [user, setUser] = useState(null); 
  
  // Load user data
  useEffect(() => {
    const savedUser = localStorage.getItem('lumina_current_user_v2');
    if (savedUser) {
      setUser(JSON.parse(savedUser));
    }
  }, []);

  // Save user data
  useEffect(() => {
    if (user) {
      localStorage.setItem('lumina_current_user_v2', JSON.stringify(user));
    } else {
      localStorage.removeItem('lumina_current_user_v2');
    }
    document.documentElement.classList.toggle('dark', darkMode);
  }, [user, darkMode]);

  const handleLogin = (name, role) => {
    if (!name.trim()) return;
    setUser({
      name: name,
      role: role,
      level: 1,
      xp: 0,
      nextLevelXp: 1000,
      streak: 0,
      savedVocab: [],
      completedLessons: [],
      quizScores: {}
    });
    setCurrentView(role === 'teacher' ? 'teacher' : 'home');
  };

  const handleLogout = () => {
    setUser(null);
  };

  const addXP = (amount) => {
    setUser(prev => {
      let newXp = prev.xp + amount;
      let newLevel = prev.level;
      let newNextXp = prev.nextLevelXp;
      while (newXp >= newNextXp) { newLevel += 1; newNextXp += 1000; }
      let newStreak = prev.streak === 0 && amount > 0 ? 1 : prev.streak;
      return { ...prev, xp: newXp, level: newLevel, nextLevelXp: newNextXp, streak: newStreak };
    });
  };

  const toggleSaveWord = (id) => {
    setUser(prev => ({
      ...prev,
      savedVocab: prev.savedVocab.includes(id) 
        ? prev.savedVocab.filter(v => v !== id) : [...prev.savedVocab, id]
    }));
  };

  // --- VIEWS ---

  // 1. LOGIN VIEW
  const LoginView = () => {
    const [inputName, setInputName] = useState('');
    const [selectedRole, setSelectedRole] = useState('student');

    return (
      <div className="min-h-screen flex items-center justify-center bg-[var(--c-seashell)] dark:bg-gray-900 p-4 animate-fade-in">
        <Card className="max-w-md w-full !p-8 text-center border-t-8 border-[#3D1534]">
          <div className="w-20 h-20 bg-[#F6E0B6] rounded-3xl flex items-center justify-center text-[#3D1534] font-bold text-5xl mx-auto shadow-lg shadow-[#F6E0B6]/30 mb-4 font-heading">E</div>
          <h1 className="text-2xl font-bold text-[#3D1534] dark:text-white leading-tight">English Studio</h1>
          <p className="text-sm text-[#A6BCC9] mb-8 font-medium">by dekyingkorket</p>

          <div className="space-y-4 text-left">
            <div>
              <label className="block text-sm font-bold text-gray-700 dark:text-gray-300 mb-2">ชื่อของคุณ (Nickname)</label>
              <input 
                type="text" 
                value={inputName}
                onChange={(e) => setInputName(e.target.value)}
                placeholder="กรอกชื่อเพื่อสร้างโปรไฟล์..." 
                className="w-full px-4 py-3 rounded-xl border-2 border-gray-200 dark:border-gray-700 bg-gray-50 dark:bg-gray-800 dark:text-white focus:border-[#3E4B8E] focus:outline-none"
              />
            </div>
            
            <div>
              <label className="block text-sm font-bold text-gray-700 dark:text-gray-300 mb-2">บทบาทผู้ใช้งาน</label>
              <div className="flex gap-2">
                <button 
                  onClick={() => setSelectedRole('student')}
                  className={`flex-1 py-3 rounded-xl font-bold border-2 transition-all ${selectedRole === 'student' ? 'border-[#3E4B8E] bg-[#3E4B8E]/10 text-[#3E4B8E]' : 'border-gray-200 text-gray-500 bg-transparent'}`}
                >
                  นักเรียน
                </button>
                <button 
                  onClick={() => setSelectedRole('teacher')}
                  className={`flex-1 py-3 rounded-xl font-bold border-2 transition-all ${selectedRole === 'teacher' ? 'border-[#3D1534] bg-[#3D1534]/10 text-[#3D1534]' : 'border-gray-200 text-gray-500 bg-transparent'}`}
                >
                  คุณครู
                </button>
              </div>
            </div>

            <button 
              onClick={() => handleLogin(inputName, selectedRole)}
              disabled={!inputName.trim()}
              className="w-full py-4 mt-4 bg-[#3E4B8E] text-white rounded-xl font-bold text-lg hover:bg-[#3D1534] transition-colors disabled:opacity-50"
            >
              เข้าสู่ระบบ / เริ่มเรียน
            </button>
          </div>
        </Card>
      </div>
    );
  };

  const HomeView = () => (
    <div className="space-y-8 animate-fade-in pb-24 md:pb-0">
      <div className="flex flex-col md:flex-row justify-between items-start md:items-center gap-4">
        <div>
          <h1 className="text-3xl md:text-4xl font-heading font-bold text-[#3D1534] dark:text-white">สวัสดี, {user.name} 👋</h1>
          <p className="text-[#3E4B8E] dark:text-[#A6BCC9] text-lg mt-1">พร้อมที่จะเรียนรู้ภาษาอังกฤษหรือยัง?</p>
        </div>
        <div className="flex gap-3 w-full md:w-auto">
          <div className="flex-1 flex items-center justify-center gap-2 text-[#3D1534] bg-white dark:bg-gray-800 dark:text-white p-3 rounded-2xl academic-shadow">
            <Star className="text-yellow-400 fill-current" size={24} />
            <span className="font-bold text-lg">เลเวล {user.level}</span>
          </div>
          <div className="flex-1 flex items-center justify-center gap-2 text-white bg-[#3E4B8E] p-3 rounded-2xl academic-shadow">
            <Flame className={`${user.streak > 0 ? 'text-orange-400' : 'text-gray-400'} fill-current`} size={24} />
            <span className="font-bold text-lg">{user.streak} วัน</span>
          </div>
        </div>
      </div>

      <Card className="bg-gradient-to-br from-[#3D1534] to-[#3E4B8E] text-white relative overflow-hidden">
        <div className="absolute top-0 right-0 w-48 h-48 bg-[#F6E0B6] rounded-full blur-3xl opacity-20 translate-x-1/2 -translate-y-1/2"></div>
        <div className="relative z-10">
          <div className="flex justify-between items-end mb-4">
            <div>
              <h3 className="text-lg opacity-90 font-medium">คะแนนประสบการณ์ (XP)</h3>
              <p className="text-3xl font-bold mt-1">{user.xp} <span className="text-lg font-normal opacity-70">/ {user.nextLevelXp}</span></p>
            </div>
            <Trophy size={48} className="text-[#F6E0B6] opacity-90" />
          </div>
          <ProgressBar progress={(user.xp / user.nextLevelXp) * 100} color="bg-[#F6E0B6]" />
          <p className="text-sm mt-3 opacity-80">สะสมอีก {user.nextLevelXp - user.xp} XP เพื่อขึ้นเลเวล {user.level + 1}!</p>
        </div>
      </Card>

      <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
        <Card onClick={() => setCurrentView('grammar')} className="group bg-white dark:bg-gray-800 border-2 border-transparent hover:border-[#3E4B8E]">
          <div className="flex justify-between items-start mb-4">
            <div className="p-3 bg-[#3E4B8E]/10 text-[#3E4B8E] rounded-xl"><BookOpen size={24}/></div>
          </div>
          <h3 className="font-bold text-xl text-[#3D1534] dark:text-white">บทเรียนและไวยากรณ์</h3>
          <p className="text-sm text-gray-600 dark:text-gray-400 mt-2">ครบทุกระดับตั้งแต่ A-Z จนถึง Advanced Tenses พร้อมแบบทดสอบท้ายบท</p>
        </Card>

        <Card onClick={() => setCurrentView('vocab')} className="group bg-white dark:bg-gray-800 border-2 border-transparent hover:border-[#3D1534]">
          <div className="flex justify-between items-start mb-4">
            <div className="p-3 bg-[#3D1534]/10 text-[#3D1534] rounded-xl"><Bookmark size={24}/></div>
          </div>
          <h3 className="font-bold text-xl text-[#3D1534] dark:text-white">คลังคำศัพท์ (Vocabulary)</h3>
          <p className="text-sm text-gray-600 dark:text-gray-400 mt-2">คำศัพท์พื้นฐานและระดับสูง พร้อมตัวอย่างการใช้งาน</p>
        </Card>
        
        <Card onClick={() => setCurrentView('conversation')} className="group bg-white dark:bg-gray-800 border-2 border-transparent hover:border-[#F6E0B6]">
          <div className="flex justify-between items-start mb-4">
            <div className="p-3 bg-[#F6E0B6]/30 text-[#8B6B2B] rounded-xl"><MessageCircle size={24}/></div>
          </div>
          <h3 className="font-bold text-xl text-[#3D1534] dark:text-white">บทสนทนา (Conversation)</h3>
          <p className="text-sm text-gray-600 dark:text-gray-400 mt-2">จำลองสถานการณ์ต่างๆ เพื่อการสื่อสารในชีวิตจริง</p>
        </Card>
      </div>
    </div>
  );

  const VocabView = () => {
    const [searchTerm, setSearchTerm] = useState("");
    const filteredVocab = VOCABULARY_DB.filter(v => 
      v.word.toLowerCase().includes(searchTerm.toLowerCase()) || v.meaning.includes(searchTerm)
    );

    return (
      <div className="space-y-6 pb-24 md:pb-0">
        <div className="flex flex-col md:flex-row justify-between items-start md:items-center gap-4">
          <h1 className="text-3xl font-heading font-bold text-[#3D1534] dark:text-white">คลังคำศัพท์</h1>
          <div className="relative w-full md:w-64">
            <Search className="absolute left-3 top-1/2 -translate-y-1/2 text-gray-400" size={20} />
            <input 
              type="text" placeholder="ค้นหาคำศัพท์..." 
              className="w-full pl-10 pr-4 py-3 rounded-2xl border-none academic-shadow focus:ring-2 focus:ring-[#3E4B8E] dark:bg-gray-800 dark:text-white"
              onChange={(e) => setSearchTerm(e.target.value)}
            />
          </div>
        </div>

        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
          {filteredVocab.map(v => (
            <Card key={v.id} className="relative overflow-hidden flex flex-col h-full group">
              <button 
                onClick={(e) => { e.stopPropagation(); toggleSaveWord(v.id); }}
                className={`absolute top-4 right-4 p-2 rounded-full ${user.savedVocab.includes(v.id) ? 'bg-[#3D1534] text-[#F6E0B6]' : 'bg-gray-100 text-gray-400 dark:bg-gray-700'}`}
              >
                <Heart size={18} className={user.savedVocab.includes(v.id) ? "fill-current" : ""} />
              </button>

              <span className={`text-xs font-bold w-fit px-3 py-1 rounded-full mb-3
                ${v.difficulty === 'Beginner' ? 'bg-green-100 text-green-700' : 
                  v.difficulty === 'Intermediate' ? 'bg-[#F6E0B6] text-[#3D1534]' : 'bg-red-100 text-red-700'}`}>
                {v.difficulty}
              </span>

              <h3 className="text-3xl font-heading font-bold text-[#3E4B8E] dark:text-white">{v.word}</h3>
              <p className="text-sm text-gray-500 mb-2">{v.ipa} • {v.pos}</p>
              <p className="text-lg font-medium text-[#3D1534] dark:text-[#A6BCC9] mb-4">{v.meaning}</p>
              
              <div className="mt-auto bg-gray-50 dark:bg-gray-900 rounded-xl p-4">
                <p className="text-sm font-medium text-gray-700 dark:text-gray-300">"{v.exampleEn}"</p>
                <p className="text-sm text-gray-500 mt-1">{v.exampleTh}</p>
              </div>

              {v.synonyms && v.synonyms.length > 0 && (
                <div className="mt-4 pt-3 border-t border-gray-100 dark:border-gray-700">
                  <span className="text-xs text-gray-400 font-bold block mb-1">คำเหมือน (Synonyms):</span>
                  <div className="flex flex-wrap gap-1">
                    {v.synonyms.map(syn => <span key={syn} className="text-xs bg-gray-100 dark:bg-gray-700 px-2 py-1 rounded-md">{syn}</span>)}
                  </div>
                </div>
              )}
            </Card>
          ))}
        </div>
      </div>
    );
  };

  const GrammarView = () => {
    const [activeModule, setActiveModule] = useState(GRAMMAR_MODULES[0]);
    const [activeLesson, setActiveLesson] = useState(GRAMMAR_MODULES[0].lessons[0]);
    const [isTakingQuiz, setIsTakingQuiz] = useState(false);
    const [quizQIndex, setQuizQIndex] = useState(0);
    const [selectedAnswer, setSelectedAnswer] = useState(null);
    const [score, setScore] = useState(0);
    const [quizFinished, setQuizFinished] = useState(false);

    useEffect(() => {
      setIsTakingQuiz(false); setQuizQIndex(0); setScore(0);
      setQuizFinished(false); setSelectedAnswer(null);
    }, [activeLesson]);

    const handleAnswer = (idx) => {
      if (selectedAnswer !== null) return;
      setSelectedAnswer(idx);
      if (idx === activeLesson.quiz[quizQIndex].ans) setScore(prev => prev + 1);
    };

    const handleNextQuestion = () => {
      if (quizQIndex < activeLesson.quiz.length - 1) {
        setQuizQIndex(prev => prev + 1); setSelectedAnswer(null);
      } else {
        setQuizFinished(true);
        if (!user.completedLessons.includes(activeLesson.id)) {
          setUser(prev => ({ 
            ...prev, 
            completedLessons: [...prev.completedLessons, activeLesson.id],
            quizScores: { ...prev.quizScores, [activeLesson.id]: score + (selectedAnswer === activeLesson.quiz[quizQIndex].ans ? 1 : 0) }
          }));
          const earnedXP = (score + (selectedAnswer === activeLesson.quiz[quizQIndex].ans ? 1 : 0)) * 10;
          addXP(earnedXP + 50); 
        }
      }
    };

    return (
      <div className="flex flex-col lg:flex-row gap-6 pb-24 md:pb-0 h-[calc(100vh-140px)]">
        <Card className="lg:w-1/3 overflow-y-auto shrink-0 flex flex-col gap-2 p-4 md:p-6 hide-scrollbar">
          <h2 className="text-xl font-heading font-bold text-[#3D1534] dark:text-white mb-4">หลักสูตรไวยากรณ์</h2>
          {GRAMMAR_MODULES.map(mod => (
            <div key={mod.id} className="mb-4">
              <h3 className="text-sm font-bold text-[#3E4B8E] dark:text-[#A6BCC9] mb-2">{mod.title}</h3>
              <div className="space-y-2">
                {mod.lessons.map(lesson => (
                  <button
                    key={lesson.id}
                    onClick={() => { setActiveModule(mod); setActiveLesson(lesson); }}
                    className={`w-full text-left p-3 rounded-xl flex items-center justify-between transition-colors
                      ${activeLesson.id === lesson.id ? 'bg-[#3E4B8E] text-white shadow-md' : 'bg-gray-50 hover:bg-gray-100 dark:bg-gray-800 dark:hover:bg-gray-700 text-gray-700 dark:text-gray-300'}`}
                  >
                    <span className="font-medium text-sm">{lesson.title}</span>
                    {user.completedLessons.includes(lesson.id) && <CheckCircle size={16} className={activeLesson.id === lesson.id ? "text-[#F6E0B6]" : "text-green-500"} />}
                  </button>
                ))}
              </div>
            </div>
          ))}
        </Card>

        <Card className="lg:w-2/3 overflow-y-auto relative p-6 md:p-10 hide-scrollbar">
          {!isTakingQuiz ? (
            <div className="max-w-2xl mx-auto space-y-8 animate-fade-in">
              <div>
                <span className="text-xs font-bold text-[#3E4B8E] bg-[#3E4B8E]/10 px-3 py-1 rounded-full">{activeModule.title}</span>
                <h1 className="text-3xl font-heading font-bold text-[#3D1534] dark:text-white mt-4">{activeLesson.title}</h1>
              </div>
              <div className="text-lg leading-relaxed text-gray-700 dark:text-gray-300 whitespace-pre-line">{activeLesson.explanation}</div>
              <div className="bg-[#FFF4EB] dark:bg-gray-900 border-l-4 border-[#3E4B8E] p-6 rounded-r-2xl">
                <h4 className="text-sm font-bold text-gray-500 mb-2">โครงสร้างที่ต้องจำ</h4>
                <p className="text-xl font-semibold text-[#3D1534] dark:text-white whitespace-pre-line">{activeLesson.structure}</p>
              </div>
              <div>
                <h3 className="text-xl font-bold text-[#3E4B8E] mb-4">ตัวอย่าง (Examples)</h3>
                <div className="space-y-3">
                  {activeLesson.examples.map((ex, i) => (
                    <div key={i} className="bg-gray-50 dark:bg-gray-800 p-4 rounded-xl border border-gray-100 dark:border-gray-700">
                      <p className="text-lg font-bold text-[#3D1534] dark:text-white">{ex.en}</p>
                      <p className="text-gray-600 dark:text-gray-400 text-sm mt-1">{ex.th}</p>
                    </div>
                  ))}
                </div>
              </div>
              <div className="bg-red-50 dark:bg-red-900/20 p-5 rounded-xl text-red-800 dark:text-red-200">
                <strong className="flex items-center gap-2 mb-1"><Flame size={18}/> จุดที่มักผิดบ่อย</strong>
                <p className="text-sm">{activeLesson.commonMistakes}</p>
              </div>
              <div className="pt-8 flex flex-col items-center border-t border-gray-100 dark:border-gray-700">
                <p className="text-gray-500 mb-4 text-sm">ทดสอบความเข้าใจ รับแต้ม XP!</p>
                <button 
                  onClick={() => setIsTakingQuiz(true)}
                  className="w-full md:w-auto px-8 py-4 bg-[#3E4B8E] text-white rounded-2xl font-bold text-lg hover:bg-[#3D1534] transition-all flex items-center justify-center gap-2"
                >
                  <PenTool size={20}/> เริ่มทำแบบทดสอบ ({activeLesson.quiz.length} ข้อ)
                </button>
              </div>
            </div>
          ) : !quizFinished ? (
            <div className="max-w-2xl mx-auto animate-fade-in">
              <button onClick={() => setIsTakingQuiz(false)} className="flex items-center gap-2 text-gray-500 mb-6 hover:text-[#3E4B8E]"><X size={20}/> ออกจากแบบทดสอบ</button>
              <div className="mb-8">
                <div className="flex justify-between text-sm font-bold text-gray-500 mb-2"><span>ข้อที่ {quizQIndex + 1} จาก {activeLesson.quiz.length}</span></div>
                <ProgressBar progress={((quizQIndex) / activeLesson.quiz.length) * 100} />
              </div>
              <h2 className="text-2xl font-bold text-[#3D1534] dark:text-white mb-6">{activeLesson.quiz[quizQIndex].q}</h2>
              <div className="space-y-3">
                {activeLesson.quiz[quizQIndex].options.map((opt, idx) => {
                  let btnStyle = "bg-gray-50 dark:bg-gray-800 border-gray-200 dark:border-gray-700 text-gray-700 dark:text-gray-200 hover:border-[#3E4B8E]";
                  if (selectedAnswer !== null) {
                    if (idx === activeLesson.quiz[quizQIndex].ans) btnStyle = "bg-green-100 border-green-500 text-green-800";
                    else if (idx === selectedAnswer) btnStyle = "bg-red-100 border-red-500 text-red-800";
                    else btnStyle = "opacity-50 border-gray-200";
                  }
                  return (
                    <button key={idx} onClick={() => handleAnswer(idx)} disabled={selectedAnswer !== null} className={`w-full text-left p-4 rounded-xl border-2 font-medium transition-all ${btnStyle}`}>
                      {opt}
                    </button>
                  );
                })}
              </div>
              {selectedAnswer !== null && (
                <div className="mt-6 p-4 rounded-xl bg-blue-50 dark:bg-blue-900/20 text-blue-800 dark:text-blue-200 animate-fade-in">
                  <p className="font-bold mb-1">{selectedAnswer === activeLesson.quiz[quizQIndex].ans ? "✅ ถูกต้อง!" : "❌ ยังไม่ถูกนะ"}</p>
                  <p className="text-sm">{activeLesson.quiz[quizQIndex].exp}</p>
                  <button onClick={handleNextQuestion} className="mt-4 w-full py-3 bg-[#3E4B8E] text-white rounded-xl font-bold">{quizQIndex < activeLesson.quiz.length - 1 ? 'ข้อต่อไป' : 'ดูคะแนนสรุป'}</button>
                </div>
              )}
            </div>
          ) : (
            <div className="flex flex-col items-center justify-center h-full text-center animate-fade-in">
              <Trophy size={80} className="text-yellow-400 mb-4" />
              <h2 className="text-3xl font-bold text-[#3D1534] dark:text-white mb-2">ทำแบบทดสอบเสร็จแล้ว!</h2>
              <p className="text-xl text-gray-600 dark:text-gray-400 mb-6">คุณทำได้ {score} / {activeLesson.quiz.length} คะแนน</p>
              <div className="bg-[#FFF4EB] dark:bg-gray-800 p-6 rounded-2xl mb-8">
                <p className="text-sm font-bold text-gray-500 mb-1">ได้รับ XP พิเศษ</p>
                <p className="text-3xl font-bold text-[#3E4B8E]">+{ (score * 10) + 50 } XP</p>
              </div>
              <button onClick={() => setCurrentView('home')} className="px-6 py-3 rounded-xl font-bold text-white bg-[#3E4B8E] hover:bg-[#3D1534]">กลับหน้าหลัก</button>
            </div>
          )}
        </Card>
      </div>
    );
  };

  const ConversationView = () => {
    const [activeConvo, setActiveConvo] = useState(CONVERSATIONS[0]);
    return (
      <div className="max-w-5xl mx-auto pb-24 md:pb-0 flex flex-col md:flex-row gap-6">
        <Card className="md:w-1/3 shrink-0 h-fit space-y-2 p-4">
          <h2 className="font-heading font-bold text-xl text-[#3D1534] dark:text-white mb-4">สถานการณ์</h2>
          {CONVERSATIONS.map(c => (
            <button key={c.id} onClick={() => setActiveConvo(c)} className={`w-full text-left p-4 rounded-xl transition-all border-2 ${activeConvo.id === c.id ? 'border-[#3E4B8E] bg-[#FFF4EB] dark:bg-gray-800' : 'border-transparent bg-gray-50 hover:bg-gray-100 dark:bg-gray-900'}`}>
              <span className="font-bold text-[#3E4B8E] block">{c.title}</span>
            </button>
          ))}
        </Card>
        <div className="md:w-2/3 space-y-6">
          <Card className="space-y-4">
            <h1 className="text-2xl font-bold text-[#3D1534] dark:text-white mb-6 border-b pb-4">{activeConvo.title}</h1>
            {activeConvo.dialogue.map((line, i) => {
              const isA = i % 2 === 0;
              return (
                <div key={i} className={`flex flex-col ${isA ? 'items-start' : 'items-end'}`}>
                  <span className="text-xs font-bold text-gray-400 mb-1 px-1">{line.speaker}</span>
                  <div className={`max-w-[85%] p-4 rounded-2xl ${isA ? 'bg-gray-100 dark:bg-gray-800 rounded-tl-sm' : 'bg-[#3E4B8E] text-white rounded-tr-sm'}`}>
                    <p className="text-lg font-medium">{line.en}</p>
                    <p className={`text-sm mt-1 pt-2 border-t ${isA ? 'border-gray-200 text-gray-500' : 'border-white/20 text-[#A6BCC9]'}`}>{line.th}</p>
                  </div>
                </div>
              )
            })}
          </Card>
          <Card className="bg-[#FFF4EB] dark:bg-gray-800">
            <h3 className="font-bold text-lg mb-3 flex items-center gap-2"><BookOpen size={20}/> คำศัพท์น่ารู้</h3>
            <div className="flex flex-wrap gap-2">
              {activeConvo.vocab.map((v, i) => <span key={i} className="bg-white dark:bg-gray-900 px-3 py-2 rounded-lg text-sm font-medium border border-[#F6E0B6]">{v}</span>)}
            </div>
          </Card>
        </div>
      </div>
    );
  };

  const DashboardView = () => (
    <div className="space-y-8 max-w-5xl mx-auto pb-24 md:pb-0 animate-fade-in">
      <div className="flex flex-col md:flex-row gap-8 items-center bg-[#3D1534] text-white p-8 rounded-3xl relative overflow-hidden">
        <div className="absolute top-0 right-0 w-64 h-64 bg-[#F6E0B6] rounded-full blur-3xl opacity-10 translate-x-1/2 -translate-y-1/2"></div>
        <div className="w-32 h-32 bg-[#A6BCC9] rounded-full flex items-center justify-center text-5xl border-4 border-white/20 z-10 font-heading">
          {user.name.charAt(0).toUpperCase()}
        </div>
        <div className="text-center md:text-left z-10">
          <h1 className="text-4xl font-heading font-bold mb-2">{user.name}</h1>
          <p className="text-[#A6BCC9] text-lg">บทบาท: นักเรียน</p>
          <div className="flex gap-4 mt-4 justify-center md:justify-start">
            <span className="bg-white/10 px-4 py-2 rounded-full font-medium">เลเวล {user.level}</span>
            <span className="bg-white/10 px-4 py-2 rounded-full font-medium">{user.xp} XP สะสม</span>
          </div>
        </div>
      </div>

      <div className="grid grid-cols-1 md:grid-cols-2 gap-8">
        <Card>
          <h2 className="text-2xl font-heading font-bold text-[#3E4B8E] mb-6 border-b pb-2">ความสำเร็จ (Achievements)</h2>
          <div className="grid grid-cols-3 gap-4">
            <Badge icon={Flame} title="เข้าเรียนต่อเนื่อง" active={user.streak > 0} />
            <Badge icon={BookOpen} title="จบบทแรก" active={user.completedLessons.length > 0} />
            <Badge icon={Trophy} title="ทำควิซได้เต็ม" active={Object.values(user.quizScores).some(s => s >= 3)} />
            <Badge icon={Heart} title="นักสะสมศัพท์" active={user.savedVocab.length > 0} />
            <Badge icon={Star} title="เลเวล 2" active={user.level >= 2} />
            <Badge icon={Award} title="จอมเวทย์ไวยากรณ์" active={user.completedLessons.length >= 7} />
          </div>
        </Card>

        <Card>
          <h2 className="text-2xl font-heading font-bold text-[#3E4B8E] mb-6 border-b pb-2">สถิติการเรียน</h2>
          <div className="space-y-6">
            <div>
              <div className="flex justify-between mb-2 text-sm font-bold text-gray-600 dark:text-gray-300">
                <span>เรียนไวยากรณ์จบแล้ว</span>
                <span>{user.completedLessons.length} บท</span>
              </div>
              <ProgressBar progress={Math.min((user.completedLessons.length / 7) * 100, 100)} color="bg-[#3E4B8E]" />
            </div>
            <div>
              <div className="flex justify-between mb-2 text-sm font-bold text-gray-600 dark:text-gray-300">
                <span>บันทึกคำศัพท์แล้ว</span>
                <span>{user.savedVocab.length} คำ</span>
              </div>
              <ProgressBar progress={Math.min((user.savedVocab.length / VOCABULARY_DB.length) * 100, 100)} color="bg-[#3D1534]" />
            </div>
          </div>
        </Card>
      </div>
    </div>
  );

  const TeacherDashboardView = () => (
    <div className="space-y-8 animate-fade-in pb-24 md:pb-0">
      <div className="flex flex-col md:flex-row justify-between items-start md:items-center gap-4 border-b-2 border-gray-100 dark:border-gray-800 pb-6">
        <div>
          <h1 className="text-3xl font-heading font-bold text-[#3D1534] dark:text-white">ระบบจัดการหลังบ้าน (Teacher Panel)</h1>
          <p className="text-gray-500 mt-1">ยินดีต้อนรับ, ครู{user.name}</p>
        </div>
      </div>
      
      <div className="grid grid-cols-1 md:grid-cols-4 gap-4">
        {[
          { label: "นักเรียนทั้งหมด", val: "1", icon: Users, color: "text-blue-500" },
          { label: "คะแนนควิซเฉลี่ย", val: "85%", icon: BarChart, color: "text-green-500" },
          { label: "นักเรียน Active", val: "1", icon: Flame, color: "text-orange-500" },
          { label: "คำศัพท์ที่นักเรียนบันทึก", val: user.savedVocab.length.toString(), icon: BookOpen, color: "text-purple-500" }
        ].map((stat, i) => (
          <Card key={i} className="flex items-center gap-4">
            <div className={`p-4 bg-gray-50 dark:bg-gray-700 rounded-2xl ${stat.color}`}><stat.icon size={28} /></div>
            <div>
              <p className="text-3xl font-bold text-gray-800 dark:text-white">{stat.val}</p>
              <p className="text-xs text-gray-500 font-bold mt-1">{stat.label}</p>
            </div>
          </Card>
        ))}
      </div>

      <Card>
        <h3 className="text-xl font-bold mb-4 text-[#3D1534] dark:text-white">รายชื่อนักเรียน (Mock Data)</h3>
        <div className="overflow-x-auto">
          <table className="w-full text-left">
            <thead>
              <tr className="text-gray-400 border-b dark:border-gray-700">
                <th className="pb-3 font-medium">ชื่อนักเรียน</th>
                <th className="pb-3 font-medium">เลเวล</th>
                <th className="pb-3 font-medium">XP สะสม</th>
                <th className="pb-3 font-medium">ความคืบหน้า</th>
              </tr>
            </thead>
            <tbody className="text-gray-700 dark:text-gray-300">
              <tr className="border-b dark:border-gray-700/50 hover:bg-gray-50 dark:hover:bg-gray-800/50">
                <td className="py-4 font-medium flex items-center gap-3">
                  <div className="w-8 h-8 rounded-full bg-[#A6BCC9] flex items-center justify-center text-white text-xs">{user.name.charAt(0)}</div>
                  {user.name} (คุณเอง)
                </td>
                <td className="py-4">Lv. {user.level}</td>
                <td className="py-4">{user.xp} XP</td>
                <td className="py-4">
                  <div className="flex items-center gap-2">
                    <div className="w-24"><ProgressBar progress={(user.completedLessons.length/7)*100} height="h-1.5" /></div>
                    <span className="text-xs">{(user.completedLessons.length/7)*100}%</span>
                  </div>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </Card>
    </div>
  );

  const NavItem = ({ id, icon: Icon, label, requireRole }) => {
    if (requireRole && user?.role !== requireRole) return null;
    const isActive = currentView === id;
    return (
      <button
        onClick={() => setCurrentView(id)}
        className={`flex flex-col md:flex-row items-center gap-1 md:gap-3 p-2 md:px-4 md:py-3 rounded-2xl transition-all duration-300 w-full md:w-auto
          ${isActive ? 'bg-[#F6E0B6] text-[#3D1534] shadow-md' : 'text-white/70 hover:text-white hover:bg-white/10'}`}
      >
        <Icon size={20} className={isActive ? 'scale-110' : ''} />
        <span className={`text-[10px] md:text-sm font-bold ${isActive ? 'block' : 'hidden md:block'}`}>{label}</span>
      </button>
    );
  };

  if (!user) return <LoginView />;

  return (
    <>
      <style>{globalStyles}</style>
      <div className="min-h-screen flex flex-col md:flex-row text-[var(--c-midnight)] dark:text-white bg-[var(--c-seashell)] dark:bg-gray-900">
        
        <nav className="fixed bottom-0 w-full md:relative md:w-64 bg-[#3D1534] md:min-h-screen z-50 rounded-t-[2rem] md:rounded-none md:rounded-r-[3rem] shadow-[0_-10px_40px_rgba(0,0,0,0.2)] flex md:flex-col p-4 md:p-6 transition-colors dark:bg-black">
          <div className="hidden md:flex flex-col items-center gap-2 mb-10 text-white text-center">
            <div className="w-14 h-14 bg-[#F6E0B6] rounded-2xl flex items-center justify-center text-[#3D1534] font-bold text-3xl shadow-lg shadow-[#F6E0B6]/20">E</div>
            <div>
              <span className="font-heading font-bold text-lg block leading-tight mt-2">English Studio</span>
            </div>
          </div>

          <div className="flex md:flex-col gap-1 w-full justify-around md:justify-start">
            {user.role === 'teacher' ? (
              <>
                <NavItem id="teacher" icon={Settings} label="แดชบอร์ดครู" />
                <NavItem id="grammar" icon={BookOpen} label="ดูบทเรียน" />
              </>
            ) : (
              <>
                <NavItem id="home" icon={Home} label="หน้าแรก" />
                <NavItem id="grammar" icon={BookOpen} label="บทเรียน" />
                <NavItem id="vocab" icon={Bookmark} label="คลังศัพท์" />
                <NavItem id="conversation" icon={MessageCircle} label="บทสนทนา" />
                <NavItem id="dashboard" icon={User} label="โปรไฟล์" />
              </>
            )}
          </div>

          <div className="hidden md:block mt-auto pt-4 border-t border-white/10 space-y-2">
            <button onClick={() => setDarkMode(!darkMode)} className="flex items-center gap-3 text-white/70 hover:text-white w-full p-3 rounded-2xl hover:bg-white/10 transition-colors">
              {darkMode ? <Sun size={18} /> : <Moon size={18} />}
              <span className="font-bold text-xs">{darkMode ? 'โหมดสว่าง' : 'โหมดกลางคืน'}</span>
            </button>
            <button onClick={handleLogout} className="flex items-center gap-3 text-red-300 hover:text-red-100 w-full p-3 rounded-2xl hover:bg-white/10 transition-colors">
              <LogOut size={18} />
              <span className="font-bold text-xs">ออกจากระบบ</span>
            </button>
          </div>
        </nav>

        <main className="flex-1 p-4 md:p-8 lg:p-10 overflow-y-auto max-h-screen">
          <div className="md:hidden flex justify-between items-center mb-6 bg-white dark:bg-gray-800 p-4 rounded-2xl shadow-sm">
             <div className="flex items-center gap-3">
              <div className="w-10 h-10 bg-[#3D1534] rounded-xl flex items-center justify-center text-[#F6E0B6] font-bold text-xl">E</div>
              <span className="font-heading font-bold text-sm">English Studio</span>
            </div>
            <div className="flex gap-2">
              <button onClick={() => setDarkMode(!darkMode)} className="p-2 rounded-full bg-gray-100 dark:bg-gray-700">
                {darkMode ? <Sun size={18} className="text-yellow-500" /> : <Moon size={18} className="text-[#3D1534]" />}
              </button>
              <button onClick={handleLogout} className="p-2 rounded-full bg-red-100 text-red-600 dark:bg-red-900/30">
                <LogOut size={18} />
              </button>
            </div>
          </div>

          <div className="max-w-6xl mx-auto h-full">
            {currentView === 'home' && <HomeView />}
            {currentView === 'grammar' && <GrammarView />}
            {currentView === 'vocab' && <VocabView />}
            {currentView === 'conversation' && <ConversationView />}
            {currentView === 'dashboard' && <DashboardView />}
            {currentView === 'teacher' && <TeacherDashboardView />}
          </div>
        </main>
      </div>
    </>
  );
}
