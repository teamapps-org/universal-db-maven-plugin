
# UniversalDB maven plugin

Maven plugin for UniversalDB

## Build configuration

Add this to your pom.xml:

```xml
    <build>
        <plugins>
            <plugin>
                <groupId>org.teamapps</groupId>
                <artifactId>universal-db-maven-plugin</artifactId>
                <version>1.1-SNAPSHOT</version>
                <executions>
                    <execution>
                        <phase>generate-sources</phase>
                        <goals>
                            <goal>generate-model</goal>
                        </goals>
                        <configuration>
                            <modelSourceDirectory>${project.basedir}/src/main/model</modelSourceDirectory>
                            <modelClasses>
                                <modelClass>Model</modelClass>
                            </modelClasses>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
```

## Create UniversalDB schema

Add a source folder e.g. `/src/main/model` and create a model class e.g. `Model`:

```java
import org.teamapps.universaldb.Database;
import org.teamapps.universaldb.Schema;
import org.teamapps.universaldb.SchemaInfoProvider;
import org.teamapps.universaldb.index.Table;

public class Model implements SchemaInfoProvider {

	public String getSchema() {
		Schema schema = Schema.create();
		Database database = schema.createDatabase("databaseName");
		Table table = database.createTable("table1");
		Table table2 = database.createTable("table2");
		table
				.addText("textColumn1")
				.addText("textColumn2")
				.addTimestamp("timestamp")
				.addEnum("enumValue", "value1", "value2", "value3")
				.addInteger("number")
				.addLong("bigNumber")
				.addReference("reference", table2, true, "backRef");

		table2
				.addText("text")
				.addReference("backRef", table, false, "reference");

		return schema.getSchema();
	}
}
```

## License

The UniversalDB maven plugin is released under version 2.0 of the [Apache License](https://www.apache.org/licenses/LICENSE-2.0).
