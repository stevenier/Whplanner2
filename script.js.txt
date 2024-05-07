// Event Listener für das Laden der Seite
document.addEventListener("DOMContentLoaded", function() {
    // Standardmäßig das erste Formular anzeigen
    showSection('register-truck');
});

// Funktion zum Anzeigen eines Abschnitts und Ausblenden der anderen
function showSection(sectionId) {
    // Alle Abschnitte ausblenden
    var sections = document.querySelectorAll('section');
    sections.forEach(function(section) {
        section.style.display = 'none';
    });

    // Den ausgewählten Abschnitt anzeigen
    var selectedSection = document.getElementById(sectionId);
    selectedSection.style.display = 'block';

    // Den aktiven Link im Header markieren
    var navLinks = document.querySelectorAll('nav ul li a');
    navLinks.forEach(function(link) {
        link.classList.remove('active');
    });

    var activeLink = document.querySelector('nav ul li a[href="#' + sectionId + '"]');
    activeLink.classList.add('active');
}

// Event Listener für die Navigation links oben
var navLinks = document.querySelectorAll('nav ul li a');
navLinks.forEach(function(link) {
    link.addEventListener('click', function(event) {
        event.preventDefault(); // Standardverhalten des Links verhindern
        var sectionId = this.getAttribute('href').substring(1); // ID des Abschnitts extrahieren
        showSection(sectionId); // Funktion zum Anzeigen des Abschnitts aufrufen
    });
});

// Event Listener für das Anmeldeformular
var registerForm = document.getElementById('truckForm');
registerForm.addEventListener('submit', function(event) {
    event.preventDefault(); // Standardverhalten des Formulars verhindern
    // Formulardaten abrufen
    var truckNumber = document.getElementById('truckNumber').value;
    var parkingSpot = document.getElementById('parkingSpot').value;
    var loadingLocation = document.getElementById('loadingLocation').value;
    var material = document.getElementById('material').value;
    var startTime = document.getElementById('startTime').value;
    // AJAX-Anfrage an den Server senden
    var xhr = new XMLHttpRequest();
    xhr.open('POST', '/submit_truck', true);
    xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
    xhr.onreadystatechange = function() {
        if (xhr.readyState === 4 && xhr.status === 200) {
            alert(xhr.responseText); // Erfolgsmeldung anzeigen
            // Daten zur Tabelle hinzufügen
            var table = document.getElementById('truckData');
            var row = table.insertRow(-1); // Neue Zeile am Ende der Tabelle einfügen
            var cell1 = row.insertCell(0);
            var cell2 = row.insertCell(1);
            var cell3 = row.insertCell(2);
            var cell4 = row.insertCell(3);
            var cell5 = row.insertCell(4);
            cell1.innerHTML = truckNumber;
            cell2.innerHTML = parkingSpot;
            cell3.innerHTML = loadingLocation;
            cell4.innerHTML = material;
            cell5.innerHTML = startTime;
        }
    };
    // Formulardaten senden
    var formData = 'truckNumber=' + encodeURIComponent(truckNumber) +
                   '&parkingSpot=' + encodeURIComponent(parkingSpot) +
                   '&loadingLocation=' + encodeURIComponent(loadingLocation) +
                   '&material=' + encodeURIComponent(material) +
                   '&startTime=' + encodeURIComponent(startTime);
    xhr.send(formData);
});
