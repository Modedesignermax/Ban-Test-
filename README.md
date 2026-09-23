package com.example.banapp

data class User(
    val id: String,
    val name: String,
    var banned: Boolean = false,
    var reason: String? = null
)

class BanManager {

    private val users = mutableListOf<User>()

    fun addUser(id: String, name: String) {
        if (users.none { it.id == id }) {
            users.add(User(id, name))
        }
    }

    fun banUser(id: String, reason: String) {
        users.find { it.id == id }?.apply {
            banned = true
            this.reason = reason
        }
    }

    fun unbanUser(id: String) {
        users.find { it.id == id }?.apply {
            banned = false
            reason = null
        }
    }

    fun isBanned(id: String): Boolean {
        return users.find { it.id == id }?.banned ?: false
    }

    fun getUsers(): List<User> {
        return users.toList()
    }
}

fun main() {

    // ==========================================
    //              BANNED BY MAX
    // ==========================================

    println()
    println("==========================================")
    println("              BANNED BY MAX")
    println("==========================================")
    println()

    val banManager = BanManager()

    // Beispielbenutzer
    banManager.addUser("001", "Max")
    banManager.addUser("002", "TestUser")

    // Benutzer bannen
    banManager.banUser(
        id = "002",
        reason = "Verstoß gegen die Regeln"
    )

    // Status prüfen
    if (banManager.isBanned("002")) {
        val user = banManager.getUsers()
            .first { it.id == "002" }

        println("🚫 BANNED")
        println("User: ${user.name}")
        println("Grund: ${user.reason}")
    }
}
