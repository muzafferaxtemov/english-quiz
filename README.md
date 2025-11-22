<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Quiz (Parsed from your text)</title>
  <style>
    body{font-family:system-ui,-apple-system,Segoe UI,Roboto,'Helvetica Neue',Arial;margin:0;background:#f6f8fa;color:#111}
    header{background:#3b82f6;color:white;padding:16px;text-align:center}
    main{max-width:900px;margin:18px auto;padding:16px}
    .q{background:white;border-radius:8px;padding:14px;margin-bottom:12px;box-shadow:0 1px 4px rgba(0,0,0,0.06)}
    .q h3{margin:0 0 8px;font-size:16px}
    .opts{display:flex;flex-direction:column;gap:6px}
    label.option{display:block;padding:8px;border-radius:6px;border:1px solid #e6eefb;cursor:pointer}
    input[type=radio]{margin-right:8px}
    .correct{background:#e6ffed;border-color:#6ee7b7}
    .wrong{background:#ffe6e6;border-color:#ff9b9b}
    .controls{display:flex;gap:8px;flex-wrap:wrap;margin:12px 0}
    button{background:#3b82f6;color:white;border:none;padding:10px 14px;border-radius:8px;cursor:pointer}
    pre#source{display:none;width:100%;height:220px}
    .score{font-weight:700;margin-top:10px}
    footer{padding:10px;text-align:center;color:#666;font-size:13px}
    .small{font-size:13px;color:#555}
  </style>
</head>
<body>
  <header>
    <h1>Quiz — Auto-parsed from your text</h1>
    <div class="small">App uses the ~ marker as the correct answer (auto-detected)</div>
  </header>
  <main>
    <div class="controls">
      <button id="renderBtn">Render Quiz</button>
      <button id="submitBtn">Submit Answers</button>
      <button id="showAnswersBtn">Show Correct Answers</button>
      <button id="resetBtn">Reset</button>
      <button id="exportBtn">Download HTML</button>
    </div>

    <div id="quiz"></div>

    <pre id="source">{{QUIZ_SOURCE}}</pre>

    <div id="result" class="score"></div>
  </main>
  <footer>
    Copy this file into your Android WebView project (assets folder) or open in a browser.
  </footer>

  <script>
  // ---------------------------
  // Paste your quiz text below between the backticks. The script will parse blocks of the form:
  // Question text { ...options... }
  // Options must be each on their own line and options starting with ~ are treated as correct.
  // ---------------------------
  const quizText = `
What is the plural form of “child”? {
#childs
#childes
#childer
~Children
}

Which word is a countable noun? {
#water
#sugar
~apple
#information
}

Which is an uncountable noun? {
~ rice
#book
#chair
#student
}

Choose the correct plural?{
#mouses
#mice
~mouse
#meese
}

Choose the uncountable noun? {
 ~advice
#cars
#boys
#trees
}

Choose the plural form? {
#tooths
#teef
~teeth
#teets
}

Which word is a common noun? {
~city
#London
#Charlie
#Africa
}

Which is an abstract noun? {
#table
~happiness
#car
#teacher
}

“Furniture” is…? {
#plural
#countable
#proper
~uncountable
}

Choose a proper noun? {
~Uzbekistan
#country
#city
#mountain
}

Which is a plural noun? {
 ~geese
#goose
#gooze
#geeses
}

Which is an uncountable noun? {
#student
#pen
#dog
~knowledge
}

What is the plural of “man”? {
~men
#mans
#manes
#menes
}

Choose the countable noun? {
~bottle
#milk
#bread
#music
}

Choose the correct plural? {
~knives
#knifes
#knifs
#knife
}

Choose the abstract noun? {
~freedom
#chair
#bottle
#phone
}

“News” is…? {
~uncountable
#plural
#countable
#adjective
}

“Information” is…? {
~uncountable
#countable
#plural
#verb
}

The plural of “foot” is…? {
 ~feet
#foots
#feets
#footes
}

Choose the correct noun? {
~luggage
#luggages
#luggagen
#lugges
}

Choose the plural form? {
~leaves
#leafs
#leav
#leaven
}

Choose the correct one? {
~water
#waters
#wateres
#watter
}

“Sugar” is…? {
~uncountable
#plural
#countable
#proper
}

Choose the plural noun? {
~women
#womans
#womenses
#woman
}

Which is a common noun? {
~ teacher
Beyoncé
China
Amazon
}

Choose the abstract noun? {
~honesty
#phone
#table
#coat
}

Choose the correct plural? {
~tomatoes
#tomatos
#tomatoses
#tomati
}

Choose uncountable noun? {
~money
#bag
#computer
#chair
}

The plural of “mouse” is…? {
~mice
#mouses
#mouse
#mees
}

Choose the correct noun?{
~education
#educations
#educate
#educated
}

Choose the plural noun? {
~children
#childs
#childes
#child’s
}

“Homework” is…? {
~uncountable
#countable
#plural
#verb
}

Choose the countable noun? {
~cup
#salt
#gold
#oil
}

The plural of “person” is…? {
~people
#persons
#persones
#peoples
}

Choose the abstract noun? {
~beauty
#shoes
#hair
#notebook
}

Choose the uncountable noun? {
#apple
~music
#bus
#class
}

The plural of “tooth” is…? {
~teeth
#tooths
#teef
#teethes
}

Choose the correct noun? {
~luggage
#lugagges
#luggages
#laggage
}

Choose the plural form? {
~babies
#babys
#babyes
#babis
}

“Milk” is…? {
~uncountable
#plural
#countable
#proper
}

Big – bigger – …. ?{
~biggest
#bigest
#most big
#more big
}

Fast – faster – …? {
#Most fast
~fastest
#fastly
#fastliest
}

Good – better – …? {
~best
#goodest
#most good
#betterest
}

Bad – worse – …? {
 ~worst
#baddest
#worser
#more bad
}

Beautiful – more beautiful – …? {
~most beautiful
#beautifulest
#beautifullest
#most beauty
}

Interesting – more interesting – …? {
~most interesting
#interestinger
#more interest
#interestfullest
}

Tall – taller – … ?{
~tallest
#most tall
#talled
#talliest
}

Short – shorter – …? {
~shortest
#most short
#shortiest
#more shorter
}

Hot – hotter – … ?{
~hottest
#hotest
#most hot
#more hot
}

Cold – colder – …? {
~coldest
#most cold
#coldiest
#colderest
}

Little – less – …? {
 ~least
#littlest
#lessest
#more little
}

Far – farther – …? {
~farthest
#furtherest
#most far
#farest
}

Easy – easier – …? {
#most easy
#easiyest
#easyest
~easiest
}

Busy – busier – …? {
~busiest
#most busy
#busyiest
#busyyest
}

Happy – happier – …? {
#most happy
~happiest
#happyest
#happiester}

Expensive – more expensive – …? {
~ most expensive
#expensiveest
#costliest
#more expensiver
}

Old – older – …? {
#most old
~oldest
#oldiest
#olderest
}

Young – younger – … ?{
 ~youngest
#most young
#youngeest
#more younger
}

Long – longer – … ?{
~ longest
#most long
#longiest
#more long
}

Slow – slower – …? {
#most slow
#slowiest
~slowest
#slowerest
}

Clever – more clever – …? {
 ~most clever
#cleverest
#more smartest
#more smartly
}

Funny – funnier – …? {
#most funny
#funnyest
#funniesti
~funniest
}

Large – larger – …? {
~ largest
#largiest
#most large
#largerest
}

Small – smaller – …? {
#most small
~smallest
#smalliest
#smallerest
}

Strong – stronger – …? {
~strongest
#strongiest
#most strong
#more stronger
}

Weak – weaker – …? {
~weakest
#most weak
#weakiest
#weakerest
}

Cheap – cheaper – …? {
#most cheap
#cheapiest
#cheaperest
~cheapest
}

Rich – richer – …? {
~ richest
#richiest
#most rich
#richerest
}

Poor – poorer – …? {
#most poor
~poorest
#poored
#poorerest
}

Thin – thinner – … ?{
~thinnest
#thinest
#most thin
#thinyest
}

Choose the imperative form?
“_ the door.” {
~ Close
#Closes
#Closing
#To close
}

“_ quiet, please.” ?{
 ~Be
#Being
#Is
#Are
}

“_ your homework!”? {
~ Do
#Does
#Doing
#To do
}

“_ late!” ?{
~ Don’t be
#Not be
#Don’t is
#Isn’t
}

“_ down!” ?{
#Sits
#Sitting
#To sit
~sit
}
“_ your name here.” ?{
#Writes
~Write
#Writing
#To writing
}

“_ your phone!”? {
~ Turn off
#Turns off
#Turning off
#To turn off
}

“_ listen!”? {
#Not
#Doesn’t
#Isn’t
~Don’t 
}

“_ slowly!” ?{
~Speak
#Speaks
#Speaking
#To speak
}

“_ me your book.” ?{
#Gives
#Giving
~Give
#To give
}

“_ to me!”? {
~ Listen
#Listening
#Listens
#To listen
}

“_ careful!”? {
~ Be
#Being
#Is
#Are
}

“_ your hands!”? {
#Washes
~Wash
#Washing
#To wash
}

“_ your room!” ?{
~ Clean
#Cleans
#Cleaning
#To clean
}

“_ here!? {
#Comes
#Coming
~Come
#To come
}

“_ this exercise.”? {
 ~Do
#Doing
#Does
#To do
}

“_ fast!”? {
#Not run
~Don’t run
#Don’t running
#Doesn’t run
}

“_ your books.” ?{
~ Open
#Opens
#Opening
#To open
}

“_ the window.”? {
~ Don’t open
#Doesn’t open
#Not open
#Isn’t open
}

“_ attention!”? {
#Pays
~Pay
#Paying
#To pay
}

She _ to school every day? {
~ goes
#go
#going
#is go
}

They _ football on Sundays? {
#plays
~play
#playing
#is play
}

I _ English.? {
~speak
#speaks
#speaking
#is speak
}

He _ TV in the evening?{
#watch
#watching
~watches
#is watch
}

Water _ at 100°C.? {
~boils
#boil
#is boiling
#boiling
}

My father _ a doctor? {
#are
#am
#be
~is
}

She _ coffee? {
~ likes
#like
#liking
#is like
}

The sun _ in the east.? {
#rise
#rising
#is rise
~rises
}

We _ to music. ?{
 listen
#listens
#listening
#is listen
}

A teacher _ in a school.? {
#work
~works
#working
#is work
}

Do you _ cats? {
~ like
#likes
#liking
#to like
}

He doesn’t _ meat? {
~ eat
#eats
#eating
#ate
}

Where _ you live? {
#does
#is
#are
~do
}

She doesn’t _ English.? {
~ speak
#speaks
#speaking
#spoke
}

My brother _ in Tashkent.? {
#live
#living
~lives
#is live
}

Birds _ in the sky? {
~fly
#flies
#flying
#is fly
}

They don’t _ early? {
~ get up
#gets up
#getting up
#got up
}

_ she work on Sundays? {
#Do
#Is
~Does
#Are
}

My mother _ delicious food? {
~ cooks
#cook
#cooking
#is cook
}

He never _ late? {
 ~comes
#come
#coming
#is come
}

We always _ tea in the morning? {
#drinks
#drinking
#is drink
~drink
}

The store _ at nine? {
~opens
#open
#opening
#is open
}

They _ English at school? {
#studies
#studying
~study
#is study
}

She _ very well?{
~ reads
#read
#reading
#is read
}

My friend _ in a bank.? {
 ~works
#work
#working
#is work
}

Do they _ in a flat? {
~ live
#lives
#living
#lived
}

I _ breakfast at 7.? {
~ have
#has
#having
#is have
}

Tom and Sam _ brothers.? {
~ are
#is
#am
#be
}

He _ to music every day.? {
~ listens
#listen
#listening
#is listen
}
She _ now.? {
~ is studying
#studies
#study
#is study
}
They _ football at the moment.? {
~ are playing
#play
#plays
#is playing
}

I _ to music now.? {
~am listening
#listen
#listening
#is listening
}
He _ dinner right now.?{
~is cooking
#cooks
#cook
#cooking
}

We _ TV at the moment.? {
 ~are watching
#watch
#watching
#is watching
}

She _ a book now? {
is reading
#reads
#read
#reading
}

They _ to school now?{
 ~are going
#go
#goes
#going
}

I _ a letter now. ?{
~ am writing
#write
#writes
#writing
}

He _ with his friends at the moment.? {
~ is talking
#talks
#talk
#is talk
}

We _ lunch now.? {
~are having
#have
#has
#having
}

The baby _ now.? {
~ is sleeping
#sleeps
#sleep
#sleeping
}

The dog _ loudly!? {
 ~ is barking
#bark
#barks
#barking
}

It _ outside.? {
~ is raining
#rains
#rain
#raining
}

My friends _ chess now? {
 ~are playing
#play
#plays
#are play
}

She _ on the phone now.? {
~ is talking
#talk
#talks
#talking
}

They _ dinner right now.? {
~are having
#have
#has
#having
}

Tom _ a picture now.? {
 ~is drawing
#draw
#drawing
#draws
}

We _ for the test now.?{
 ~are preparing
#prepare
#preparing
#are prepare
}

She _ her room now.? {
~ is cleaning
#cleans
#cleaning
#is clean
}

I _ a cake now.? {
~ am baking
#bake
#baking
#am bake
}

They _ home now.? {
~ are going
#go
#goes
#going
}

The teacher _ the lesson now.? {
~ is explaining
#explains
#explain
#explaining
}

He _ the piano now? {
~ is playing
#play
#plays
#playing
}

You _ too fast!?{
~ are speaking
#speak
#speaks
#speaking
}

My mother _ dinner now.? {
~ is cooking
#cooks
#cooking
#is cook
}

The kids _ in the yard now.? {
 ~are playing
#play
#plays
#are play
}

I _ my homework right now.? {
~ am doing
#do
#does
vdoing
}

He _ a car now.? {
~ is driving
#drives
#drive
#is drive
}

I _ a student.? {
~ am
#is
#are
#be
}

She _ a doctor.? {
~ is
#are
#am
#be
}

They _ at home.? {
 ~are
#is
#am
#be
}

We _ friends.?{
 ~are
#is
#am
#be
}

He _ in the room.? {
 ~is
#are
#am
#be
}

It _ cold today.? {
 ~is
#are
#am
#be
}

The cats _ hungry.? {
~ are
#is
#am
#be
}

My brother _ tall.? {
~ is
#are
#am
#be
}

You _ very kind.? {
~ are
#is
#am
#be
}

I _ from Uzbekistan?. {
 ~am
#is
#are
#be
}

The weather _ nice.? {
~ is
#are
#be
#am
}

My parents _ teachers.? {
~ are
#is
#am
#be
}

My mother _ angry.? {
 ~is
#are
#am
#be
}

The book _ interesting.? {
 ~is
#are
#am
#be
}
The students _ in class.? {
~ are
#is
#am
#be
}

English _ easy.? {
~ is
#are
#am
#be
}

Her name _ Sara.? {
~ is
#are
#am
#be
}

The park _ beautiful.? {
~ is
#are
#am
#be
}

We _ ready?. {
 ~are
#is
#am
#be
}

My friend _ at work.?{
~ is
#are
#am
#be
}

They _ busy today.? {
~are
#is
#am
#be
}

I _ not tired.? {
~ am
#is
#are
#be
}

She _ at home now.? {
~ is
#are
#am
#be
}

You _ my best friend.? {
 ~are
#is
#am
#be
}

He _ not here.? {
~ is
#are
#am
#be
}

_ a book on the table? {
 ~There is
#There are
#Is there
#There be
}

_ two cats in the room.? {
 ~There are
#There is
#Are there
#There be
}

_ a park near my house.? {
~ There is
#There are
#Is there
#It is
}

_ many students in the class.? {
~ There are
#There is
#It is
#They are
}

_ a pen in my bag. {
 ~There is
#There are
#Is there
#It is
}

_ three windows in the kitchen. ?{
~ There are
#There is
#Are there
#It is
}

_ a dog in the yard. ?{
~ There is
#There are
#It is
#There be
}

_ any problems? {
 ~Are there
#Is there
#There are
#There is
}

_ a new student today.? {
 ~There is
#There are
#It is
#He is
}

_ five apples on the table.? {
~ There are
#There is
#Are there
#It is
}

_ a big supermarket here. ?{
 ~There is
#There are
#It is
#They are
}

_ some water in the bottle.? {
~There is
#There are
#It are
#It is
}

_ two chairs near the window.? {
~There are
#There is
#Are there
#They are
}

_ a bus stop near here? {
~ Is there
#Are there
#There is
#It is
}

_ many people outside.? {
 ~There are
#There is
#It are
#It is
}

_ a picture on the wall.? {
 ~There is
#There are
#It is
#Is there
}

_ any milk in the fridge? {
~ Is there
#Are there
#There are
#There is
}

_ two beds in the room.? {
There are
#There is
#It is
#They are
}

_ a car outside.? {
~There is
#There are
#It are
#Is car
}

_ any juice? {
~ Is there
#Are there
#There is
#It is
}

_ a phone on the table.? {
~ There is
#There are
#It is
#Is there are
}

_ shops near my house.? {
~There are
#There is
#It are
#They is
}

_ an apple in the bag.? {
 ~There is
#There are
#It is
#Are there
}

_ three boys in the yard.? {
 ~There are
#There is
#They are
#It are
}

_ a movie tonight? {
~ Is there
#Are there
#It is
#There are
}


She ___ a student.?{
~ is 
# am 
# are 
# be
}

I ____ happy.? { 
~ am 
# is 
# are 
# be
}

We ____ friends.? {
~ are 
# is 
# am 
# be
}

The cat ___ black.? {
~ is 
# am 
# are 
# be
}

They ____ at home.? {
~ are 
# is 
# am 
# be
}

You ____ tall.? {
~ are 
# is 
# am 
# be
}




He ____ nice.? { 
~ is 
# am 
# are 
# be
}

My mother ____ a doctor.? {
~ is 
# am 
# are 
# be
}

The books ____ heavy. ?{
~ are 
# is 
# am 
# be
}

I ____ ready.? {
~ am 
# is 
# are 
# be
}

Someone from the USA is _______.? {
~ American 
# Germany 
# China 
# Spain
}

The capital of France is Paris. People there are _______.? {
~ French 
# Brazil 
# Japan 
# Australia
}

If you live in Italy, you are _______.? {
~ Italian 
# Russia 
# Mexico 
# Korea
}

People from Canada speak English and French. They are _______.?{
~ Canadian 
# India 
# Egypt
# Greece
}

She comes from China. She is _______.? {
~ Chinese 
# The UK 
# Argentina 
# Portugal
}

He is English. His country is _______.? {
~ England 
# France 
# Spain 
# Italy
}

I am from Japan. I am _______.? {
~ Japanese 
# Greek 
# Mexican 
# Russian
}

They live in Brazil. They are _______.? {
~ Brazilian 
# Canadian 
# French 
# Chinese
}

We are from Germany. We are _______.? {
~ German 
# Italian 
# American 
# English
}





People from Spain are _______.? {
~ Spanish 
# Australian 
# Indian 
# German
}

I have __ dog.? {
~ a 
# an 
# the 
# is
}

She eats __ apple every day.? {
~ an 
# a 
# the 
# its
}

I see __ bird in 
the tree.? {
~ a 
# an 
# the 
# has
}

We need __ umbrella.? {
~ an 
# a 
# the 
# are
}

That is __ big house.? {
~ a 
# an 
# the 
# not
}

He wants __ ice cream.? {
~ an 
# a 
# the  
# my
}

This is __ book.? {
~ a 
# an 
# the 
# his
}

I bought __ hour ago.? {
~ an 
# a 
# the 
# from
}

She drives __ red car.? {
~ a 
# an 
# the 
# in
}

I saw __ elephant at the zoo.? {
~ an 
# a 
# the 
# on
}

___ like to read.? {
~ I 
# Me 
# My 
# Mine
}

____ is a kind person.? {
~ She 
# Her 
# Hers 
# We
}

___ is playing soccer.? {
~ He 
# Him 
# His 
# They

The ball is big. ___ is red.? {
~ It 
# Its
# They
# Them
}

___ are going to the park.? {
~ We 
# Us 
# Our 
# Yours
}

____ are a good singer.? {
~ You 
# Your 
# Yours 
# Him
}

___ are watching a movie.? {
~ They 
# Them 
# Their 
# She
}
My brother and ___ went to the store.? {
~ I 
# me 
# my 
# mine
}

My parents are here. ____ arrived early.? {
~ They 
# Them 
# Their 
# We
}

Look at ___ apple in my hand. (singular, near)? { 
~ this 
# that 
# these 
# those
}

____ car far away is new. (singular, far) ?{
~ That 
# This 
# These
# Those
}

____ shoes on my feet are comfortable. (plural, near) ?{
~ These 
# Those 
# That 
# This
}

____ clouds in the sky look grey. (plural, far)? {
~ Those 
# This 
# That 
# These
}

____ is my chair. (singular, near)? {
~ This 
# That 
# Those 
# These
}

____ is the moon tonight. (singular, far)? {
~ That 
# This 
# These 
# Those
}

____ flowers smell good. (plural, near)? {
~ These 
# Those 
# That 
# This
}
`;

  // Put source into pre for transparency
  document.getElementById('source').textContent = quizText;

  function parseQuiz(text){
    const regex = /([\s\S]*?)\{([\s\S]*?)\}/g;
    let m; const questions = [];
    while((m = regex.exec(text)) !== null){
      const qRaw = m[1].trim().replace(/\n$/,'');
      const optsRaw = m[2].trim();
      if(!qRaw) continue;
      const qText = qRaw.replace(/\n/g,' ').trim();
      const optLines = optsRaw.split(/\n/).map(s=>s.trim()).filter(Boolean);
      const options = [];
      let correctIndex = null;
      optLines.forEach((line,i)=>{
        const marker = line[0];
        let text = line;
        if(marker === '~' || marker === '#') text = line.substring(1).trim();
        // also handle lines without marker
        if(line.startsWith('~')){ correctIndex = i; }
        options.push({text: text, raw: line});
      });
      // if no marked correct option, try to find one that begins with tilde in spaced form
      questions.push({q: qText, options, correctIndex});
    }
    return questions;
  }

  function render(questions){
    const container = document.getElementById('quiz');
    container.innerHTML='';
    questions.forEach((qq,qi)=>{
      const div = document.createElement('div'); div.className='q';
      const h = document.createElement('h3'); h.textContent = (qi+1)+'. '+qq.q; div.appendChild(h);
      const opts = document.createElement('div'); opts.className='opts';
      qq.options.forEach((opt,oi)=>{
        const id = `q${qi}_o${oi}`;
        const label = document.createElement('label'); label.className='option';
        const input = document.createElement('input'); input.type='radio'; input.name='q'+qi; input.id=id; input.value=oi;
        label.appendChild(input);
        const span = document.createElement('span'); span.innerHTML = opt.text.replace(/^~/,'');
        label.appendChild(span);
        opts.appendChild(label);
      });
      div.appendChild(opts);
      container.appendChild(div);
    });
  }

  function grade(questions){
    let correct = 0; let total = questions.length;
    const container = document.getElementById('quiz');
    questions.forEach((q,qi)=>{
      const selected = document.querySelector(`input[name=q${qi}]:checked`);
      const labels = container.children[qi].querySelectorAll('label.option');
      // clear classes
      labels.forEach(l=>{ l.classList.remove('correct','wrong'); });
      const correctIdx = q.correctIndex;
      // mark correct label
      if(correctIdx!==null && labels[correctIdx]) labels[correctIdx].classList.add('correct');
      if(selected){
        const got = parseInt(selected.value,10);
        if(got===correctIdx){ correct++; }
        else { labels[got].c
