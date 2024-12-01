
# 🎵 Spotify Advanced SQL Project  

## Overview  
This project involves analyzing a Spotify dataset containing various attributes about tracks, albums, and artists using **SQL**. The project’s goals are to:  
1. Normalize a denormalized dataset for efficient SQL operations.  
2. Execute SQL queries categorized by complexity (easy, medium, and advanced).  
3. Optimize query performance to enhance database efficiency.  

---

## Dataset  
The dataset contains rich attributes for advanced SQL analysis, including:  

### Key Columns:  
- **Metadata**: Artist, Track, Album, Album Type (e.g., Single or Album).  
- **Audio Properties**: Danceability, Energy, Tempo, Valence, Liveness, Acousticness, etc.  
- **Streaming Data**: Views, Likes, Comments, Licensed Status, Official Video Flag.  
- **Derived Metrics**: Energy-to-Liveness Ratio, Most Played Platform.  

---

## Project Steps  

### 1. Data Exploration  
Thoroughly explored the dataset to understand its structure and identify key attributes for meaningful analysis.  

---

### 2. Querying the Data  
SQL queries were designed at varying difficulty levels to progressively develop skills:  

#### **Easy Queries**  
- Retrieve tracks with over 1 billion streams.  
- List all albums and their respective artists.  

#### **Medium Queries**  
- Calculate the average danceability for tracks in each album.  
- Identify the top 5 tracks with the highest energy values.  

#### **Advanced Queries**  
- Find the top 3 most-viewed tracks for each artist using **window functions**:  
    ```sql
    SELECT artist, track, 
           SUM(views) AS total_views,
           DENSE_RANK() OVER (PARTITION BY artist ORDER BY SUM(views) DESC) AS rank
    FROM spotify
    GROUP BY artist, track
    ORDER BY artist, rank;
    ```  
- Use a **WITH clause** to calculate the energy difference for tracks in each album:  
    ```sql
    WITH cte AS (
        SELECT album, 
               MAX(energy) AS highest_energy, 
               MIN(energy) AS lowest_energy
        FROM spotify
        GROUP BY album
    )
    SELECT album, 
           (highest_energy - lowest_energy) AS energy_diff
    FROM cte
    ORDER BY energy_diff DESC;
    ```  

---

### 3. Query Optimization  
#### **Initial Performance Analysis**  
- Used the `EXPLAIN` function to analyze query performance.  

#### **Optimization Strategy**  
- Added an index on the `artist` column to accelerate row retrieval:  
    ```sql
    CREATE INDEX idx_artist ON spotify(artist);
    ```  

#### **Performance Results**  
- **Before Optimization:** Query Execution Time = ~7 ms.  
- **After Optimization:** Query Execution Time = ~0.153 ms.  

---

## Key Insights  
1. **Top 3 Most-Viewed Tracks**: Identified using **window functions**.  
2. **Liveness Score Analysis**: Found tracks where the liveness score exceeds the average.  
3. **Platform Analysis**: Artists with the highest total likes for tracks primarily streamed on YouTube.  
4. **Album Type Metrics**: Calculated average audio properties grouped by album type.  

---

## Tools and Technologies  
- **Database**: PostgreSQL  
- **Query Techniques**: Aggregations, Window Functions, Subqueries, CTEs  
- **Performance Tools**: `EXPLAIN`, Indexing  

---

## How to Run the Project  

### Prerequisites  
- PostgreSQL installed on your system.  
- A SQL editor like **pgAdmin** or **DBeaver**.  

### Steps  
1. Clone this repository:  
    ```bash
    git clone https://github.com/yourusername/Spotify-Advanced-SQL-Project.git
    ```  
2. Set up the database schema using the provided script:  
    ```bash
    psql -U <your_username> -d <database_name> -f sql_scripts/create_tables.sql
    ```  
3. Import the dataset:  
    ```bash
    COPY spotify FROM '<path_to_dataset>/spotify_data.csv' DELIMITER ',' CSV HEADER;
    ```  
4. Execute the SQL scripts (`sql_scripts/`) to run queries and analyze the data.  
5. Optimize queries using indexing techniques.  

---

## Results and Visuals  
- **Optimized Query Performance**: Significant improvements achieved through indexing.  
- **Key Query Outputs**:  
    - Top Tracks and Artists by Popularity.  
    - Average Audio Metrics by Album Type.  
- **Visual Performance Comparison**: Graphs showcasing execution time before and after optimization.  

---

## Next Steps  
1. **Visualization**: Use Tableau or Power BI to create interactive dashboards.  
2. **Scalability**: Expand the dataset to test SQL performance on larger data.  
3. **Automation**: Automate query execution for periodic insights.  

---

## Contributing  
Contributions are welcome! Please feel free to:  
- Fork this repository.  
- Create a feature branch.  
- Submit a pull request.  

---

## License  
This project is licensed under the MIT License.  
