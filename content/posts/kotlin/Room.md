---
Title: Room (Databases)
Date: 2022-08-01
Tags: 
- kotlin
Summary: Notes on how to use the Room package to interact with databases in kotlin
---
## `Entity`
Using Room, tables are created with via an Entity. A class tagged `@Entity` then uses each property to represent a column in the table
```kotlin
@Entity(tableName = "word_table")
data class Word (@PrimaryKey @ColumnInfo(name = "word") val word: String)
```
- When you want the name of the table to be different than the name of the class, use the `tableName` option of `@Entity`
- Every entity needs a primary key, notated with `@PrimaryKey`
- Similar to `tableName`, if you want the column name to not match the property name, use the `name` option of `@ColumnInfo`
- You cannot have private properties in an entity

## DAO
DAO stands for 'data access object', and allows us to match up SQL queries to method calls. It must be an interface or abstract class. The included annotations for within a DAO include:
- `@Query`, which requires a string query as a parameter
    ```kotlin
    @Query("SELECT * FROM word_table ORDER BY word ASC")
    fun getAlphabetizedWords(): List<Word>
    ```
- `@Insert`, which does not need a query
    - specify what to do about conflicts using the `onConflict` parameter. `OnConflictStrategy.IGNORE` tells the table to ignore an insert if the entry already exists
    ```kotlin
    @Insert(onConflict = OnConflictStrategy.IGNORE)
    suspend fun insert(word: Word)
    ```
> Sidenote: `suspend` tells the system that this function may hang while it waits for a response, and is related to coroutines
- `@Delete`
- `@Update`

## `Room`
The actual class that represents the database. Annotated with the `@Database` tag. Must be abstract.
```kotlin
// Annotates class to be a Room Database with a table (entity) of the Word class
@Database(entities = arrayOf(Word::class), version = 1, exportSchema = false)
abstract class WordRoomDatabase : RoomDatabase(){

    abstract fun wordDao(): WordDao

    companion object {
        // Singleton prevents multiple instances of database opening
        // at the same time.
        @Volatile
        private var INSTANCE: WordRoomDatabase? = null
        fun getDatabase(context: Context): WordRoomDatabase {
            return INSTANCE ?: synchronized(this) {
                val instance = Room.databaseBuilder(
                    context.applicationContext,
                    WordRoomDatabase::class.java,
                    "word_database"
                ).build()
                INSTANCE = instance
                instance
            }
        }
    }
}
```

