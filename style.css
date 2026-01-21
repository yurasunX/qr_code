let currentTab = 'url';
let currentQRData = '';

// Tab switching
document.querySelectorAll('.tab').forEach(tab => {
    tab.addEventListener('click', () => {
        const tabName = tab.dataset.tab;
        switchTab(tabName);
    });
});

function switchTab(tabName) {
    currentTab = tabName;
    
    // Update active tab
    document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
    document.querySelector(`[data-tab="${tabName}"]`).classList.add('active');
    
    // Update active content
    document.querySelectorAll('.tab-content').forEach(c => c.classList.remove('active'));
    document.getElementById(`${tabName}-form`).classList.add('active');
    
    // Update section title
    const titles = {
        'url': 'Введите URL',
        'text': 'Введите текст',
        'contact': 'Контактная информация'
    };
    document.getElementById('section-title').textContent = titles[tabName];
    
    // Regenerate QR code
    generateQR();
}

// Input listeners
document.getElementById('url-input').addEventListener('input', generateQR);
document.getElementById('text-input').addEventListener('input', generateQR);

document.querySelectorAll('#contact-form input').forEach(input => {
    input.addEventListener('input', generateQR);
});

function generateQR() {
    let data = '';
    
    switch(currentTab) {
        case 'url':
            const urlInput = document.getElementById('url-input').value.trim();
            if (urlInput) {
                data = urlInput.startsWith('http://') || urlInput.startsWith('https://') 
                    ? urlInput 
                    : 'https://' + urlInput;
            }
            break;
            
        case 'text':
            data = document.getElementById('text-input').value;
            break;
            
        case 'contact':
            const firstName = document.getElementById('first-name').value;
            const lastName = document.getElementById('last-name').value;
            const phone = document.getElementById('phone').value;
            const email = document.getElementById('email').value;
            const organization = document.getElementById('organization').value;
            const website = document.getElementById('website').value;
            
            if (firstName || lastName || phone || email) {
                data = `BEGIN:VCARD
VERSION:3.0
FN:${firstName} ${lastName}
N:${lastName};${firstName};;;
ORG:${organization}
TEL:${phone}
EMAIL:${email}
URL:${website}
END:VCARD`;
            }
            break;
    }
    
    currentQRData = data;
    
    const qrContainer = document.getElementById('qr-code');
    const actionButtons = document.getElementById('action-buttons');
    const qrDataSection = document.getElementById('qr-data-section');
    const qrDataText = document.getElementById('qr-data-text');
    
    if (data.trim()) {
        // Clear previous QR
        qrContainer.innerHTML = '';
        
        // Create canvas
        const canvas = document.createElement('canvas');
        qrContainer.appendChild(canvas);
        
        // Generate QR code
        try {
            const qr = new QRious({
                element: canvas,
                value: data,
                size: 300,
                background: 'white',
                foreground: 'black',
                level: 'M'
            });
            
            // Add description
            const description = document.createElement('p');
            description.style.cssText = 'font-size: 0.875rem; color: #5eead4; margin-top: 1rem;';
            description.textContent = 'Отсканируйте этот QR-код вашим устройством';
            qrContainer.appendChild(description);
            
            // Show buttons and data
            actionButtons.classList.remove('hidden');
            qrDataSection.classList.remove('hidden');
            qrDataText.textContent = data;
            
        } catch (error) {
            console.error('Error generating QR code:', error);
            // Fallback to Google Charts API
            const img = document.createElement('img');
            img.src = `https://chart.googleapis.com/chart?chs=300x300&cht=qr&chl=${encodeURIComponent(data)}&choe=UTF-8`;
            img.alt = 'Generated QR Code';
            qrContainer.appendChild(img);
            
            actionButtons.classList.remove('hidden');
            qrDataSection.classList.remove('hidden');
            qrDataText.textContent = data;
        }
        
    } else {
        // Show placeholder
        qrContainer.innerHTML = `
            <svg class="qr-icon-large" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <rect x="3" y="3" width="7" height="7"></rect>
                <rect x="14" y="3" width="7" height="7"></rect>
                <rect x="14" y="14" width="7" height="7"></rect>
                <rect x="3" y="14" width="7" height="7"></rect>
            </svg>
            <p>Заполните форму для генерации QR-кода</p>
        `;
        
        actionButtons.classList.add('hidden');
        qrDataSection.classList.add('hidden');
    }
}

function downloadQR() {
    if (!currentQRData) return;
    
    const canvas = document.querySelector('#qr-code canvas');
    const img = document.querySelector('#qr-code img');
    
    if (canvas) {
        const link = document.createElement('a');
        link.download = `qr-code-${currentTab}.png`;
        link.href = canvas.toDataURL();
        link.click();
    } else if (img) {
        const link = document.createElement('a');
        link.download = `qr-code-${currentTab}.png`;
        link.href = img.src;
        link.click();
    }
}

function copyData() {
    if (!currentQRData) return;
    
    navigator.clipboard.writeText(currentQRData).then(() => {
        const copyText = document.getElementById('copy-text');
        const copyIcon = document.getElementById('copy-icon');
        
        copyText.textContent = 'Скопировано!';
        copyIcon.innerHTML = '<polyline points="20 6 9 17 4 12"></polyline>';
        
        setTimeout(() => {
            copyText.textContent = 'Копировать данные';
            copyIcon.innerHTML = `
                <rect x="9" y="9" width="13" height="13" rx="2" ry="2"></rect>
                <path d="M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1"></path>
            `;
        }, 2000);
    }).catch(err => {
        console.error('Failed to copy:', err);
    });
}

function clearForm() {
    document.getElementById('url-input').value = '';
    document.getElementById('text-input').value = '';
    document.getElementById('first-name').value = '';
    document.getElementById('last-name').value = '';
    document.getElementById('phone').value = '';
    document.getElementById('email').value = '';
    document.getElementById('organization').value = '';
    document.getElementById('website').value = '';
    
    generateQR();
}