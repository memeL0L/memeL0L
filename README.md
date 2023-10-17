import time
import pyautogui

# Set the delay between clicks (in seconds)
delay = 1.6

# Set the pixel color to look for
trigger_color = (255, 0, 0)

# Start the triggerbot loop
while True:

    # Get the current pixel color under the cursor
    pixel_color = pyautogui.pixel(pyautogui.position())

    # If the current pixel color matches the trigger color, click the mouse
    if pixel_color == trigger_color:
        pyautogui.click()

    # Wait for the delay before checking the pixel color again
    time.sleep(delay)
import time
import pyautogui
import keyboard

# Set the delay between clicks (in seconds)
delay = 0.1

# Set the key to press to use the totem of undying
totem_key = "f"

# Set the hotkey to toggle the autototem script
autototem_key = "x"

# Start the autototem loop
while True:

    # If the autototem script is enabled, check the player's health
    if keyboard.is_pressed(autototem_key):

        # If the player is dead, press the totem key
        if keyboard.is_pressed("space"):
            pyautogui.press(totem_key)

    # Wait for the delay before checking the player's health again
    time.sleep(delay)
import time
import pyautogui
import keyboard

# Set the delay between checks (in seconds)
delay = 0.1

# Set the key to press to toggle the playeresp script
playeresp_key = "f3"

# Define a function to draw a box around a player
def draw_player_box(x, y, z):
    pyautogui.moveTo(x, y)
    pyautogui.click(button="left")
    pyautogui.moveTo(x + 10, y)
    pyautogui.click(button="left")
    pyautogui.moveTo(x + 10, y + 10)
    pyautogui.click(button="left")
    pyautogui.moveTo(x, y + 10)
    pyautogui.click(button="left")
    pyautogui.moveTo(x, y)

# Start the playeresp loop
while True:

    # If the playeresp script is enabled, check for players nearby
    if keyboard.is_pressed(playeresp_key):

        # Get a list of all players nearby
        players = pyautogui.locateAllOnScreen("player.png", grayscale=True)

        # Draw a box around each player
        for player in players:
            draw_player_box(player.x, player.y, player.z)

    # Wait for the delay before checking for players nearby again
    time.sleep(delay)
plugins {
    id 'fabric-loom' version '0.12.+'
    id 'maven-publish'
}

sourceCompatibility = JavaVersion.VERSION_17
targetCompatibility = JavaVersion.VERSION_17

archivesBaseName = project.archives_base_name
version = project.mod_version
group = project.maven_group

repositories {
    maven { url "https://api.modrinth.com/maven" }
}

dependencies {
    //to change the versions see the gradle.properties file
    minecraft "com.mojang:minecraft:${project.minecraft_version}"
    mappings "net.fabricmc🧶${project.yarn_mappings}:v2"
    modImplementation "net.fabricmc:fabric-loader:${project.loader_version}"

    modImplementation "net.fabricmc.fabric-api:fabric-api:${project.fabric_version}"

    modImplementation "maven.modrinth:midnightlib:${project.midnightlib_version}"
    include "maven.modrinth:midnightlib:${project.midnightlib_version}"
}

processResources {
    inputs.property "version", project.version

    filesMatching("fabric.mod.json") {
        expand "version": project.version
    }
}

// ensure that the encoding is set to UTF-8, no matter what the system default is
// this fixes some edge cases with special characters not displaying correctly
// see http://yodaconditions.net/blog/fix-for-java-file-encoding-problems-with-gradle.html
tasks.withType(JavaCompile).configureEach {
    it.options.encoding = "UTF-8"
  it.options.release.set(17)
}

// Loom will automatically attach sourcesJar to a RemapSourcesJar task and to the "build" task
// if it is present.
// If you remove this task, sources will not be generated.
java {
    withSourcesJar()
}

jar {
    from "LICENSE"
}

// configure the maven publication
publishing {
    publications {
        mavenJava(MavenPublication) {
            from components.java
        }
    }

    // select the repositories you want to publish to
    repositories {
        // uncomment to publish to the local maven
        // mavenLocal()
    }
}\
plugins {
	id 'fabric-loom' version '1.2-SNAPSHOT'
	id 'maven-publish'
}

sourceCompatibility = JavaVersion.VERSION_17
targetCompatibility = JavaVersion.VERSION_17

archivesBaseName = project.archives_base_name
version = project.mod_version
group = project.maven_group

loom {
	accessWidenerPath = file("src/main/resources/thunderhack.accesswidener")
}

repositories {
	maven {
		name = "jitpack.io"
		url = "https://jitpack.io"
	}
	maven {
		name = 'swt-repo'
		url = "https://maven-eclipse.github.io/maven"
	}
	maven {
		name = "meteor-maven"
		url = "https://maven.meteordev.org/releases"
	}
	repositories {
		maven {
			name = 'Big booty Mods'
			url = 'https://maven.ladysnake.org/releases'
			content {
				includeGroup 'io.github.ladysnake'
				includeGroup 'org.ladysnake'
				includeGroupByRegex 'dev\\.onyxstudios.*'
			}
		}
	}
	mavenCentral()
	jcenter()
}

configurations {
	libImpl
	modImpl
}

dependencies {
	minecraft "com.mojang:minecraft:${project.minecraft_version}"
	mappings "net.fabricmc:yarn:${project.yarn_mappings}:v2"
	modImplementation "net.fabricmc:fabric-loader:${project.loader_version}"
	modImplementation "net.fabricmc.fabric-api:fabric-api:${project.fabric_version}"
	
	modImpl("org.ladysnake:satin:${project.satin_version}")
	libImpl("com.github.meteordevelopment:orbit:${project.orbit_version}")

	configurations.libImpl.dependencies.each {
		implementation(it)
	}
	configurations.modImpl.dependencies.each {
		modImplementation(it)
		include(it)
	}
}

processResources {
	inputs.property "version", project.version

	filesMatching("fabric.mod.json") {
		expand "version": project.version
	}
}

tasks.withType(JavaCompile).configureEach {
	it.options.release = 17
}

java {
	withSourcesJar()
}

jar {
	from("LICENSE") {
		rename { "${it}_${project.archivesBaseName}"}
	}
	from {
		configurations.libImpl.collect { it.isDirectory() ? it : zipTree(it) }
	}
	duplicatesStrategy(DuplicatesStrategy.EXCLUDE)
}

publishing {
	publications {
		mavenJava(MavenPublication) {
			from components.java
		}
	}

	repositories {
	}
}
