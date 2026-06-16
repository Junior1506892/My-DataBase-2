-- ============================================================
-- GO WILD WILDLIFE PARK DATABASE
-- Complete Database with ALL Tables
-- Compatible with MySQL and SQLite
-- ============================================================

-- ============================================================
-- STEP 1: DROP EXISTING TABLES (Clean start)
-- ============================================================
DROP TABLE IF EXISTS Animal;
DROP TABLE IF EXISTS Species;
DROP TABLE IF EXISTS Diet;
DROP TABLE IF EXISTS Keeper;
DROP TABLE IF EXISTS Enclosure;

-- ============================================================
-- STEP 2: CREATE ALL TABLES
-- ============================================================

-- ------------------------------------------------------------
-- TABLE 1: Species
-- Stores information about each animal species
-- ------------------------------------------------------------
CREATE TABLE Species (
    SpeciesID VARCHAR(10) PRIMARY KEY,
    SpeciesType VARCHAR(50) NOT NULL,
    SpeciesGroup VARCHAR(30) NOT NULL,
    Lifestyle VARCHAR(30) NOT NULL,
    ConservationStatus VARCHAR(30) NOT NULL,
    -- Validation Rules
    CHECK (ConservationStatus IN ('Least Concern', 'Threatened', 'Vulnerable', 'Endangered', 'Critically Endangered')),
    CHECK (SpeciesType != ''),
    CHECK (SpeciesGroup != '')
);

-- ------------------------------------------------------------
-- TABLE 2: Diet
-- Stores dietary information
-- ------------------------------------------------------------
CREATE TABLE Diet (
    DietID VARCHAR(10) PRIMARY KEY,
    DietType VARCHAR(20) NOT NULL,
    NoOfFeedsPerDay INTEGER NOT NULL,
    -- Validation Rules
    CHECK (NoOfFeedsPerDay >= 1 AND NoOfFeedsPerDay <= 10),
    CHECK (DietType IN ('Omnivore', 'Herbivore', 'Carnivore')),
    CHECK (DietType != '')
);

-- ------------------------------------------------------------
-- TABLE 3: Keeper
-- Stores staff/keeper information
-- ------------------------------------------------------------
CREATE TABLE Keeper (
    KeeperID VARCHAR(10) PRIMARY KEY,
    KeeperName VARCHAR(50) NOT NULL,
    KeeperDoB DATE NOT NULL,
    KeeperRank VARCHAR(20) NOT NULL,
    -- Validation Rules
    CHECK (KeeperRank IN ('Senior', 'Standard', 'Junior')),
    CHECK (KeeperName != ''),
    -- Format check: Date must be valid (handled by DATE type)
    -- Age validation: Must be at least 16 years old
    CHECK (strftime('%Y', 'now') - strftime('%Y', KeeperDoB) >= 16)
);

-- ------------------------------------------------------------
-- TABLE 4: Enclosure
-- Stores enclosure information
-- ------------------------------------------------------------
CREATE TABLE Enclosure (
    EnclosureID VARCHAR(10) PRIMARY KEY,
    EnclosureType VARCHAR(30) NOT NULL,
    EnclosureLocation VARCHAR(20) NOT NULL,
    -- Validation Rules
    CHECK (EnclosureLocation IN ('North', 'South', 'East', 'West')),
    CHECK (EnclosureType != '')
);

-- ------------------------------------------------------------
-- TABLE 5: Animal (MAIN TABLE)
-- Stores individual animal records
-- Links to all other tables via Foreign Keys
-- ------------------------------------------------------------
CREATE TABLE Animal (
    AnimalID VARCHAR(10) PRIMARY KEY,
    AnimalName VARCHAR(50) NOT NULL,
    Gender VARCHAR(6) NOT NULL,
    YearOfArrival INTEGER NOT NULL,
    SpeciesID VARCHAR(10) NOT NULL,
    DietID VARCHAR(10) NOT NULL,
    KeeperID VARCHAR(10) NOT NULL,
    EnclosureID VARCHAR(10) NOT NULL,
    -- Foreign Keys (Referential Integrity)
    FOREIGN KEY (SpeciesID) REFERENCES Species(SpeciesID),
    FOREIGN KEY (DietID) REFERENCES Diet(DietID),
    FOREIGN KEY (KeeperID) REFERENCES Keeper(KeeperID),
    FOREIGN KEY (EnclosureID) REFERENCES Enclosure(EnclosureID),
    -- Validation Rules
    CHECK (Gender IN ('M', 'F')),
    CHECK (YearOfArrival >= 1900 AND YearOfArrival <= strftime('%Y', 'now')),
    CHECK (AnimalName != '')
);

-- ============================================================
-- STEP 3: INSERT DATA INTO ALL TABLES
-- ============================================================

-- ------------------------------------------------------------
-- Insert Species Data
-- ------------------------------------------------------------
INSERT INTO Species (SpeciesID, SpeciesType, SpeciesGroup, Lifestyle, ConservationStatus) VALUES
('S3', 'Gorilla', 'Mammal', 'Troop', 'Threatened'),
('S4', 'Orang-utan', 'Mammal', 'Solitary', 'Critically Endangered'),
('S6', 'Rhinoceros', 'Mammal', 'Solitary', 'Critically Endangered'),
('S7', 'Crocodile', 'Reptile', 'Social', 'Vulnerable'),
('S8', 'Elephant', 'Mammal', 'Herd', 'Threatened'),
('S9', 'Armadillo', 'Mammal', 'Solitary', 'Endangered'),
('S10', 'Giant Tortoise', 'Reptile', 'Herd', 'Vulnerable'),
('S11', 'Lion', 'Mammal', 'Pride', 'Vulnerable'),
('S12', 'Raccoon', 'Mammal', 'Solitary', 'Least Concern'),
('S13', 'Leopard', 'Mammal', 'Solitary', 'Threatened'),
('S14', 'Chinchilla', 'Mammal', 'Solitary', 'Endangered'),
('S15', 'Tamarin', 'Mammal', 'Troop', 'Critically Endangered'),
('S16', 'Penguin', 'Bird', 'Group', 'Threatened'),
('S17', 'Sea Turtle', 'Reptile', 'Solitary', 'Endangered'),
('S18', 'Sloth', 'Mammal', 'Solitary', 'Endangered'),
('S19', 'Kakapo', 'Bird', 'Solitary', 'Endangered'),
('S20', 'Hippopotamus', 'Mammal', 'Herd', 'Vulnerable');

-- ------------------------------------------------------------
-- Insert Diet Data
-- ------------------------------------------------------------
INSERT INTO Diet (DietID, DietType, NoOfFeedsPerDay) VALUES
('D1', 'Omnivore', 6),
('D2', 'Herbivore', 6),
('D3', 'Carnivore', 4);

-- ------------------------------------------------------------
-- Insert Keeper Data
-- ------------------------------------------------------------
INSERT INTO Keeper (KeeperID, KeeperName, KeeperDoB, KeeperRank) VALUES
('K1', 'Dave', '1964-06-18', 'Senior'),
('K2', 'Kayden', '1985-01-21', 'Junior'),
('K3', 'Suki', '1998-08-09', 'Standard'),
('K4', 'Temi', '2000-04-16', 'Senior');

-- ------------------------------------------------------------
-- Insert Enclosure Data
-- ------------------------------------------------------------
INSERT INTO Enclosure (EnclosureID, EnclosureType, EnclosureLocation) VALUES
('E1', 'Moat', 'North'),
('E2', 'Glass', 'North'),
('E3', 'Fence', 'South'),
('E4', 'Walled', 'South'),
('E5', 'Pen', 'South'),
('E2W', 'Glass', 'West'),
('E3W', 'Fence', 'West'),
('E4W', 'Walled', 'West'),
('E2E', 'Glass', 'East'),
('E3E', 'Fence', 'East'),
('E4E', 'Walled', 'East'),
('E5E', 'Pen', 'East');

-- ------------------------------------------------------------
-- Insert Animal Data (Main Table - 40+ records)
-- ------------------------------------------------------------
INSERT INTO Animal (AnimalID, AnimalName, Gender, YearOfArrival, SpeciesID, DietID, KeeperID, EnclosureID) VALUES
('A3', 'Geoffrey', 'M', 2018, 'S3', 'D1', 'K1', 'E2'),
('A4', 'Oliver', 'M', 2011, 'S4', 'D1', 'K1', 'E1'),
('A6', 'Roger', 'M', 2000, 'S6', 'D2', 'K2', 'E3'),
('A7', 'Clive', 'M', 2013, 'S7', 'D3', 'K2', 'E3'),
('A8', 'Eddie', 'M', 2016, 'S8', 'D2', 'K2', 'E4'),
('A9', 'Arnie', 'M', 2012, 'S9', 'D1', 'K2', 'E5'),
('A10', 'Gavin', 'M', 2015, 'S10', 'D2', 'K2', 'E5'),
('A11', 'Lucy', 'F', 2011, 'S11', 'D3', 'K3', 'E4E'),
('A12', 'Robbie', 'M', 2017, 'S12', 'D1', 'K3', 'E5E'),
('A13', 'Laura', 'F', 2018, 'S13', 'D3', 'K3', 'E3E'),
('A14', 'Casey', 'F', 2013, 'S14', 'D2', 'K3', 'E5E'),
('A15', 'Trevor', 'M', 2000, 'S15', 'D1', 'K3', 'E3E'),
('A16', 'Polly', 'F', 2017, 'S16', 'D1', 'K4', 'E2W'),
('A17', 'Sarah', 'F', 2015, 'S17', 'D1', 'K4', 'E2W'),
('A18', 'Stan', 'M', 2018, 'S18', 'D1', 'K4', 'E3W'),
('A19', 'Kara', 'F', 2001, 'S19', 'D2', 'K4', 'E4W'),
('A20', 'Henry', 'M', 2003, 'S20', 'D2', 'K4', 'E3W'),
('A22', 'Eliza', 'F', 2003, 'S8', 'D2', 'K2', 'E4'),
('A23', 'George', 'M', 2000, 'S3', 'D1', 'K1', 'E2'),
('A24', 'Carlos', 'M', 2017, 'S7', 'D3', 'K2', 'E3'),
('A25', 'Lenie', 'F', 2015, 'S11', 'D3', 'K3', 'E4E'),
('A26', 'Roberta', 'F', 2018, 'S12', 'D1', 'K3', 'E5E'),
('A27', 'Peter', 'M', 2001, 'S16', 'D1', 'K4', 'E2W'),
('A28', 'Percy', 'M', 2003, 'S16', 'D1', 'K4', 'E2W'),
('A29', 'Petal', 'F', 2003, 'S16', 'D1', 'K4', 'E2W'),
('A30', 'Sammie', 'F', 2013, 'S18', 'D1', 'K4', 'E3W'),
('A31', 'Lionel', 'M', 2016, 'S11', 'D3', 'K3', 'E4E'),
('A32', 'Gertrude', 'F', 2012, 'S3', 'D1', 'K1', 'E2'),
('A33', 'Olive', 'F', 2015, 'S4', 'D1', 'K1', 'E1'),
('A34', 'Ossie', 'M', 2011, 'S4', 'D1', 'K1', 'E1'),
('A35', 'Lena', 'F', 2017, 'S13', 'D3', 'K3', 'E3E'),
('A36', 'Rommy', 'F', 2018, 'S6', 'D2', 'K2', 'E3'),
('A37', 'Tulisa', 'F', 2013, 'S15', 'D1', 'K3', 'E3E'),
('A38', 'Chrissie', 'F', 2000, 'S7', 'D3', 'K2', 'E3'),
('A39', 'Elsie', 'F', 2017, 'S8', 'D2', 'K2', 'E4'),
('A40', 'Colin', 'M', 2015, 'S7', 'D3', 'K2', 'E3'),
('A41', 'Hattie', 'F', 2018, 'S20', 'D2', 'K4', 'E3W'),
('A42', 'Robbie', 'M', 2017, 'S6', 'D2', 'K2', 'E3'),
('A43', 'Luna', 'F', 2018, 'S11', 'D3', 'K3', 'E4E'),
('A44', 'Rebbi', 'M', 2013, 'S12', 'D1', 'K3', 'E5E'),
('A45', 'Penni', 'F', 2000, 'S16', 'D1', 'K4', 'E2W'),
('A46', 'Emmie', 'F', 2000, 'S8', 'D2', 'K2', 'E4'),
('A47', 'Lope', 'M', 2017, 'S13', 'D3', 'K3', 'E3E'),
('A48', 'Cressida', 'F', 2015, 'S14', 'D2', 'K3', 'E5E'),
('A49', 'Tommy', 'M', 2018, 'S15', 'D1', 'K3', 'E3E'),
('A50', 'Gareth', 'M', 2017, 'S3', 'D1', 'K1', 'E2');

-- ============================================================
-- STEP 4: ALL REQUIRED QUERIES (Tasks 3 & 4)
-- ============================================================

-- ------------------------------------------------------------
-- QUERY 1: Alphabetical list of keepers (ID, Name, Rank)
-- ------------------------------------------------------------
SELECT '=== QUERY 1: Alphabetical List of Keepers ===' AS '';
SELECT KeeperID AS 'Keeper ID', 
       KeeperName AS 'Keeper Name', 
       KeeperRank AS 'Rank'
FROM Keeper
ORDER BY KeeperName ASC;

-- ------------------------------------------------------------
-- QUERY 2: Number of animals in each type of enclosure
-- ------------------------------------------------------------
SELECT '=== QUERY 2: Animals per Enclosure Type ===' AS '';
SELECT e.EnclosureType AS 'Enclosure Type',
       COUNT(a.AnimalID) AS 'Number of Animals'
FROM Enclosure e
LEFT JOIN Animal a ON e.EnclosureID = a.EnclosureID
GROUP BY e.EnclosureID, e.EnclosureType
ORDER BY COUNT(a.AnimalID) DESC;

-- ------------------------------------------------------------
-- QUERY 3: Parameter query for keeper's rank
-- (Change 'Senior' to 'Standard' or 'Junior' as needed)
-- ------------------------------------------------------------
SELECT '=== QUERY 3: Keepers by Rank (Parameter Query) ===' AS '';
SELECT KeeperName AS 'Keeper Name', 
       KeeperDoB AS 'Date of Birth',
       KeeperRank AS 'Rank'
FROM Keeper
WHERE KeeperRank = 'Senior'  -- ← Change this parameter
ORDER BY KeeperName;

-- ------------------------------------------------------------
-- QUERY 4: Species with more than 3 feeds per day
-- ------------------------------------------------------------
SELECT '=== QUERY 4: Species with >3 Feeds Per Day ===' AS '';
SELECT s.SpeciesType AS 'Species',
       d.NoOfFeedsPerDay AS 'Feeds Per Day',
       COUNT(a.AnimalID) AS 'Total Animals'
FROM Species s
JOIN Animal a ON s.SpeciesID = a.SpeciesID
JOIN Diet d ON a.DietID = d.DietID
WHERE d.NoOfFeedsPerDay > 3
GROUP BY s.SpeciesID, s.SpeciesType, d.NoOfFeedsPerDay
ORDER BY COUNT(a.AnimalID) DESC;

-- ------------------------------------------------------------
-- QUERY 5: Critically Endangered Omnivores
-- ------------------------------------------------------------
SELECT '=== QUERY 5: Critically Endangered Omnivores ===' AS '';
SELECT a.AnimalID AS 'Animal ID',
       a.YearOfArrival AS 'Arrival Year',
       s.SpeciesID AS 'Species ID',
       a.KeeperID AS 'Keeper ID',
       a.AnimalName AS 'Animal Name'
FROM Animal a
JOIN Species s ON a.SpeciesID = s.SpeciesID
JOIN Diet d ON a.DietID = d.DietID
WHERE d.DietType = 'Omnivore' 
  AND s.ConservationStatus = 'Critically Endangered'
ORDER BY a.AnimalID;

-- ------------------------------------------------------------
-- QUERY 6: Animals supervised by Dave and Temi
-- ------------------------------------------------------------
SELECT '=== QUERY 6: Animals by Dave and Temi ===' AS '';
SELECT k.KeeperName AS 'Keeper',
       a.AnimalID AS 'Animal ID',
       a.AnimalName AS 'Animal Name',
       s.SpeciesType AS 'Species',
       a.Gender AS 'Gender'
FROM Animal a
JOIN Keeper k ON a.KeeperID = k.KeeperID
JOIN Species s ON a.SpeciesID = s.SpeciesID
WHERE k.KeeperName IN ('Dave', 'Temi')
ORDER BY k.KeeperName, a.AnimalName;

-- ------------------------------------------------------------
-- QUERY 7: Number of animals per keeper (with total)
-- ------------------------------------------------------------
SELECT '=== QUERY 7: Animals per Keeper ===' AS '';
SELECT k.KeeperName AS 'Keeper',
       COUNT(a.AnimalID) AS 'Animals Cared For'
FROM Keeper k
LEFT JOIN Animal a ON k.KeeperID = a.KeeperID
GROUP BY k.KeeperID, k.KeeperName
UNION ALL
SELECT 'TOTAL' AS 'Keeper',
       COUNT(AnimalID) AS 'Animals Cared For'
FROM Animal
ORDER BY Keeper;

-- ============================================================
-- STEP 5: VIEWS FOR REPORTING
-- ============================================================

-- Complete Animal Record View
DROP VIEW IF EXISTS vw_AnimalFullRecord;
CREATE VIEW vw_AnimalFullRecord AS
SELECT a.AnimalID AS 'Animal ID',
       a.AnimalName AS 'Animal Name',
       a.Gender AS 'Gender',
       a.YearOfArrival AS 'Arrival Year',
       s.SpeciesType AS 'Species',
       s.SpeciesGroup AS 'Group',
       s.Lifestyle AS 'Lifestyle',
       s.ConservationStatus AS 'Conservation Status',
       d.DietType AS 'Diet Type',
       d.NoOfFeedsPerDay AS 'Feeds Per Day',
       k.KeeperName AS 'Keeper',
       k.KeeperRank AS 'Keeper Rank',
       e.EnclosureType AS 'Enclosure Type',
       e.EnclosureLocation AS 'Location'
FROM Animal a
JOIN Species s ON a.SpeciesID = s.SpeciesID
JOIN Diet d ON a.DietID = d.DietID
JOIN Keeper k ON a.KeeperID = k.KeeperID
JOIN Enclosure e ON a.EnclosureID = e.EnclosureID;

-- ============================================================
-- STEP 6: DATA VALIDATION (Check referential integrity)
-- ============================================================
SELECT '=== DATA VALIDATION CHECKS ===' AS '';
SELECT 'Animals without valid Species' AS 'Issue', 
       COUNT(*) AS 'Count'
FROM Animal a
LEFT JOIN Species s ON a.SpeciesID = s.SpeciesID
WHERE s.SpeciesID IS NULL
UNION ALL
SELECT 'Animals without valid Diet', 
       COUNT(*)
FROM Animal a
LEFT JOIN Diet d ON a.DietID = d.DietID
WHERE d.DietID IS NULL
UNION ALL
SELECT 'Animals without valid Keeper', 
       COUNT(*)
FROM Animal a
LEFT JOIN Keeper k ON a.KeeperID = k.KeeperID
WHERE k.KeeperID IS NULL
UNION ALL
SELECT 'Animals without valid Enclosure', 
       COUNT(*)
FROM Animal a
LEFT JOIN Enclosure e ON a.EnclosureID = e.EnclosureID
WHERE e.EnclosureID IS NULL;

-- ============================================================
-- STEP 7: INDEXES FOR PERFORMANCE
-- ============================================================
CREATE INDEX IF NOT EXISTS idx_animal_species ON Animal(SpeciesID);
CREATE INDEX IF NOT EXISTS idx_animal_diet ON Animal(DietID);
CREATE INDEX IF NOT EXISTS idx_animal_keeper ON Animal(KeeperID);
CREATE INDEX IF NOT EXISTS idx_animal_enclosure ON Animal(EnclosureID);

-- ============================================================
-- STEP 8: VIEW ALL TABLES (Show what was created)
-- ============================================================
SELECT '=== LIST OF ALL TABLES ===' AS '';
SELECT name AS 'Table Name' 
FROM sqlite_master 
WHERE type='table' 
ORDER BY name;

-- ============================================================
-- STEP 9: SAMPLE DATA (Show record counts)
-- ============================================================
SELECT '=== TABLE RECORD COUNTS ===' AS '';
SELECT 'Species' AS 'Table', COUNT(*) AS 'Records' FROM Species
UNION ALL
SELECT 'Diet', COUNT(*) FROM Diet
UNION ALL
SELECT 'Keeper', COUNT(*) FROM Keeper
UNION ALL
SELECT 'Enclosure', COUNT(*) FROM Enclosure
UNION ALL
SELECT 'Animal', COUNT(*) FROM Animal;

-- ============================================================
-- END OF DATABASE SCRIPT
-- ============================================================
