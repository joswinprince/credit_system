
## Database for credit system 

### Table
```
CREATE TABLE Talent (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) UNIQUE NOT NULL
);

CREATE TABLE Learning_Item (
    id INT AUTO_INCREMENT PRIMARY KEY,
    talent_id INT NOT NULL,
    name VARCHAR(255) NOT NULL,
    FOREIGN KEY (talent_id) REFERENCES Talent(id) ON DELETE CASCADE,
    UNIQUE (talent_id, name) -- Prevent duplicate subjects within a module
);

CREATE TABLE User_Progress (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL, -- Assuming users exist in another table
    talent_id INT NOT NULL,
    learning_item_id INT NOT NULL,
    completed BOOLEAN DEFAULT FALSE,
    FOREIGN KEY (talent_id) REFERENCES Talent(id) ON DELETE CASCADE,
    FOREIGN KEY (learning_item_id) REFERENCES Learning_Item(id) ON DELETE CASCADE,
    UNIQUE (user_id, learning_item_id) -- Prevent duplicate progress tracking
);


```
Sample data
```
-- Inserting a learning module
INSERT INTO Talent (name) VALUES ('Data Science');

-- Inserting Learning Items
INSERT INTO Learning_Item (talent_id, name) VALUES 
(1, 'Machine Learning'),
(1, 'Deep Learning'),
(1, 'Data Visualization');

-- Tracking User Progress
INSERT INTO User_Progress (user_id, talent_id, learning_item_id, completed) VALUES 
(1, 1, 1, FALSE),
(1, 1, 2, FALSE),
(1, 1, 3, FALSE);

```

Calculate progress dynamically
```
SELECT 
    t.id AS talent_id,
    t.name AS talent_name,
    COUNT(li.id) AS total_items,
    COUNT(up.id) AS completed_items,
    IFNULL((COUNT(up.id) / NULLIF(COUNT(li.id), 0)) * 100, 0) AS progress_percentage
FROM Talent t
LEFT JOIN Learning_Item li ON t.id = li.talent_id
LEFT JOIN User_Progress up ON li.id = up.learning_item_id AND up.user_id = 1 AND up.completed = TRUE
WHERE t.id = 1;

```

updating user progress
```
UPDATE User_Progress 
SET completed = TRUE 
WHERE user_id = 1 AND learning_item_id = 2;

```

Removing a learninG item
```
DELETE FROM Learning_Item WHERE id = 2;

```

Automatiaclly insert new progress for New learning item
```
INSERT INTO Learning_Item (talent_id, name) VALUES (1, 'Data Engineering');

INSERT INTO User_Progress (user_id, talent_id, learning_item_id, completed)
SELECT DISTINCT 1, li.talent_id, li.id, FALSE 
FROM Learning_Item li 
WHERE li.name = 'Data Engineering';

```
