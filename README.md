#include <iostream>
#include <string>
#include <regex>
#include <stdexcept>
#include <mysql/mysql.h>  // MySQL Connector/C++

// ─────────────────────────────────────────
//  Utility: Sanitisation
// ─────────────────────────────────────────
std::string stripHTML(const std::string& input) {
    return std::regex_replace(input, std::regex("<[^>]*>"), "");
}

std::string trim(const std::string& s) {
    size_t start = s.find_first_not_of(" \t\r\n");
    size_t end   = s.find_last_not_of(" \t\r\n");
    return (start == std::string::npos) ? "" : s.substr(start, end - start + 1);
}

std::string sanitise(const std::string& input) {
    return trim(stripHTML(input));
}

// ─────────────────────────────────────────
//  Utility: Validation
// ─────────────────────────────────────────
bool isValidRating(int rating) {
    return rating >= 1 && rating <= 5;
}

bool isValidSubjectCode(const std::string& code) {
    // e.g. "CS101", "MATH202"
    return std::regex_match(code, std::regex("^[A-Z]{2,6}[0-9]{2,4}$"));
}

bool isEmpty(const std::string& s) {
    return trim(s).empty();
}

// ─────────────────────────────────────────
//  Feedback Data Structure
// ─────────────────────────────────────────
struct Feedback {
    std::string studentName;
    std::string subjectCode;
    std::string comments;
    int         rating;
};

// ─────────────────────────────────────────
//  Analytics Result Structure
// ─────────────────────────────────────────
struct Analytics {
    int    totalFeedback;
    double averageRating;
    int    totalRatings;
};
