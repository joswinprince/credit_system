
## Database for credit system 
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
