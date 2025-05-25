1. Explain the Primary Key and Foreign Key concepts in PostgreSQL.
উত্তর:
PostgreSQL একটি ওপেন সোর্স এবং অবজেক্ট-রিলেশনাল ডেটাবেস ম্যানেজমেন্ট সিস্টেম (ORDBMS)। এটি অনেক উন্নত ফিচার প্রদান করে যেমন ট্রানজ্যাকশন ম্যানেজমেন্ট, ভিউস, ট্রিগারস, স্টোরড প্রসিডিউরস, এবং কাস্টম ডেটা টাইপস। PostgreSQL ACID (Atomicity, Consistency, Isolation, Durability) কমপ্লায়েন্ট, যার মানে এটি খুব নির্ভরযোগ্য ও নিরাপদ ডেটা স্টোর করতে পারে।

উদাহরণস্বরূপ, যদি আপনি একটি ওয়াইল্ডলাইফ কনজারভেশন সিস্টেম তৈরি করেন যেখানে রেঞ্জার, প্রাণী প্রজাতি ও তাদের দর্শনের তথ্য সংরক্ষণ করতে হয়, PostgreSQL সেই ধরণের জটিল রিলেশনাল ডেটা ম্যানেজ করার জন্য খুবই উপযুক্ত।

Primary Key হচ্ছে একটি বা একাধিক কলাম যা প্রতিটি সারিকে ইউনিকভাবে সনাক্ত করে। এটি ডুপ্লিকেট বা NULL মান গ্রহণ করে না।
Foreign Key হচ্ছে এমন একটি কলাম বা কলামসমষ্টি যা অন্য একটি টেবিলের Primary Key এর সাথে সম্পর্ক তৈরি করে। এটি দুইটি টেবিলের মধ্যে রিলেশন বা সংযোগ প্রতিষ্ঠা করে।

উদাহরণস্বরূপ:

-- Primary Key
CREATE TABLE species (
    species_id SERIAL PRIMARY KEY,
    common_name VARCHAR(100)
);

-- Foreign Key
CREATE TABLE sightings (
    sighting_id SERIAL PRIMARY KEY,
    species_id INT REFERENCES species(species_id)
);
এখানে sightings টেবিলের species_id কলামটি species টেবিলের species_id এর উপর নির্ভরশীল, মানে এটিকে Foreign Key বলা হয়।

2. What is the difference between the VARCHAR and CHAR data types?
উত্তর:
VARCHAR(n) এবং CHAR(n) দুটি স্ট্রিং টাইপ হলেও তাদের মধ্যে মূল পার্থক্য হলো:

VARCHAR(n) একটি পরিবর্তনশীল দৈর্ঘ্যের স্ট্রিং। এটি যতটুকু দরকার ততটুকু স্পেস ব্যবহার করে এবং বাকি জায়গা নষ্ট করে না।

CHAR(n) একটি নির্দিষ্ট দৈর্ঘ্যের স্ট্রিং। যদি কম অক্ষরের ডেটা থাকে, তাহলে বাকি জায়গা ফাঁকা (padding) দিয়ে পূর্ণ করা হয়।

উদাহরণ:

-- VARCHAR stores only what's needed
name VARCHAR(10) = 'Tiger' 

-- CHAR always uses fixed space
name CHAR(10) = 'Tiger     ' 
সাধারণত VARCHAR বেশি ব্যবহারযোগ্য কারণ এটি বেশি ফ্লেক্সিবল এবং স্পেস বাঁচায়।

3. Explain the purpose of the WHERE clause in a SELECT statement.
উত্তর:
WHERE ক্লজ ব্যবহার করা হয় SELECT, UPDATE, অথবা DELETE স্টেটমেন্টে নির্দিষ্ট শর্ত অনুযায়ী ডেটা ফিল্টার করার জন্য। এটি নির্ধারণ করে কোন কোন রেকর্ডে কাজটি প্রযোজ্য হবে।

উদাহরণ:

-- Show all endangered species
SELECT * FROM species
WHERE conservation_status = 'Endangered';
এখানে WHERE ক্লজ শুধুমাত্র সেইসব প্রজাতিকে নির্বাচন করবে যেগুলোর সংরক্ষণ অবস্থা 'Endangered'।

4. What is the significance of the JOIN operation, and how does it work in PostgreSQL?
উত্তর:
JOIN ব্যবহার করে একাধিক টেবিলের মধ্যে সম্পর্ক স্থাপন করে একটি কম্বাইন্ড রেজাল্ট তৈরি করা যায়। সাধারণত Primary Key ও Foreign Key এর উপর ভিত্তি করে JOIN কাজ করে।

প্রধান JOIN গুলোর মধ্যে রয়েছে:

INNER JOIN → শুধুমাত্র ম্যাচিং রেকর্ড দেয়।

LEFT JOIN → বাম দিকের সব রেকর্ড এবং মিল থাকলে ডান দিকের রেকর্ড দেয়।

RIGHT JOIN → ডান দিকের সব রেকর্ড এবং মিল থাকলে বাম দিকের রেকর্ড দেয়।

উদাহরণ:

-- দেখাও কোন প্রজাতি কোথায় দেখা গেছে
SELECT s.common_name, g.location
FROM species s
JOIN sightings g ON s.species_id = g.species_id;
এখানে species ও sightings টেবিল একসাথে যুক্ত করে প্রতিটি প্রজাতি কোথায় দেখা গেছে তা দেখানো হয়েছে।

5. How can you calculate aggregate functions like COUNT(), SUM(), and AVG() in PostgreSQL?
উত্তর:
PostgreSQL-এ অ্যাগ্রিগেট ফাংশন ব্যবহার করে টেবিলের একাধিক রেকর্ডের উপর গাণিতিক বিশ্লেষণ করা যায়। নিচে তিনটি গুরুত্বপূর্ণ অ্যাগ্রিগেট ফাংশনের ব্যাখ্যা দেয়া হল:

COUNT() → রেকর্ডের সংখ্যা গণনা করে

SUM() → নির্দিষ্ট ফিল্ডের যোগফল বের করে

AVG() → গড় মান বের করে

উদাহরণ:

-- মোট কতটি sighting হয়েছে
SELECT COUNT(*) FROM sightings;

-- কোন রেঞ্জার কতটি sighting করেছে
SELECT ranger_id, COUNT(*) as total_sightings
FROM sightings
GROUP BY ranger_id;
এই ফাংশনগুলো ডেটা বিশ্লেষণে অত্যন্ত গুরুত্বপূর্ণ, যেমন কোন রেঞ্জার কত বেশি প্রাণী দেখেছে তা বোঝা যায়।



