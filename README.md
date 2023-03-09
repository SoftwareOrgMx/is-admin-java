# is-admin-java
Check if the Operating System is running in Admin privilegies or not


```java
import java.io.PrintStream;
import java.util.prefs.Preferences;

/**
 *
 * @author SoftwareOrgMX
 */
public class Mains {

    public static void main(String[] args) {
        System.out.println("Is Admin: " + isAdmin());
    }

    public static boolean isAdmin() {

        Preferences prefs = Preferences.systemRoot();
        PrintStream systemErr = System.err;
        synchronized (systemErr) {    // better synchroize to avoid problems with other threads that access System.err
            System.setErr(null);
            try {
                prefs.put("foo", "bar"); // SecurityException on Windows
                prefs.remove("foo");
                prefs.flush(); // BackingStoreException on Linux
                return true;
            } catch (Exception e) {
                return false;
            } finally {
                System.setErr(systemErr);
            }
        }
    }
}

```
