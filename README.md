sqlpp11-connector-sqlite
========================

A C++ wrapper for sqlite3 meant to be used in combination with sqlpp11.

Sample Code:
------------
See for instance tests/SampleTest.cpp

```C++
namespace sql = sqlpp::sqlite3;
int main() {
    sql::connection_config config;
    config.path_to_database = ":memory:";
    config.flags = SQLITE_OPEN_READWRITE | SQLITE_OPEN_CREATE;
    config.debug = true;

    sql::connection db(config);
    std::cerr << __FILE__ << ": " << __LINE__ << std::endl;
    db.execute("CREATE TABLE tab_sample (\
        alpha bigint(20) DEFAULT NULL,\
        beta varchar(255) DEFAULT NULL,\
        gamma bool DEFAULT NULL\
        )");

    TabSample tab;
    // explicit all_of(tab)
    for(const auto& row : db(select(all_of(tab)).from(tab).where(true)))
    {
        int64_t alpha = row.alpha;
        std::string beta = row.beta;
        bool gamma = row.gamma;
    };
```

Requirements:
-------------
__Compiler:__
sqlpp11-connector-sqlite3 makes use of C++11 and requires a recent compiler and STL. The following compilers are known to compile the test programs:

  * clang-3.2 on Ubuntu-12.4
  * g++-4.8 on Ubuntu-12.4

__Libraries:__

  * sqlpp11-connector-sqlite3 is meant to be used with sqlpp11 (https://github.com/rbock/sqlpp11).
  * sqlpp11 requires date.h (https://github.com/HowardHinnant/date).
  * libsqlite3, version 3.7.11 or later is required to use multi-row insert. Older versions (e.g. 3.7.9) work fine otherwise.
