// Funciones principales del sitio web de Yanbal Beauty

// Variables globales
let isAdminMode = false;
let testimonialSwiper = null;

// Inicialización cuando el DOM está cargado
document.addEventListener('DOMContentLoaded', function() {
    initializeSwiper();
    initializeAnimations();
    loadAdminSettings();
});

// Función para inicializar el Swiper de testimonios
function initializeSwiper() {
    testimonialSwiper = new Swiper('.testimonios-swiper', {
        loop: true,
        autoplay: {
            delay: 5000,
            disableOnInteraction: false,
        },
        pagination: {
            el: '.swiper-pagination',
            clickable: true,
        },
        breakpoints: {
            640: {
                slidesPerView: 1,
                spaceBetween: 20,
            },
            768: {
                slidesPerView: 2,
                spaceBetween: 30,
            },
            1024: {
                slidesPerView: 3,
                spaceBetween: 40,
            },
        },
    });
}

// Función para inicializar animaciones GSAP
function initializeAnimations() {
    // Animación de entrada para las tarjetas de beneficios
    gsap.from('.card-hover', {
        duration: 1,
        y: 50,
        opacity: 0,
        stagger: 0.2,
        ease: 'power3.out',
        scrollTrigger: {
            trigger: '#beneficios',
            start: 'top 80%',
        }
    });

    // Animación de entrada para las imágenes de la galería
    gsap.from('.gallery-item', {
        duration: 0.8,
        scale: 0.8,
        opacity: 0,
        stagger: 0.1,
        ease: 'back.out(1.7)',
        scrollTrigger: {
            trigger: '#galeria',
            start: 'top 80%',
        }
    });
}

// Función para hacer scroll al formulario
function scrollToForm() {
    document.getElementById('formulario').scrollIntoView({
        behavior: 'smooth',
        block: 'start'
    });
}

// Función para manejar el envío del formulario
function handleFormSubmit(event) {
    event.preventDefault();
    
    const formData = new FormData(event.target);
    const data = Object.fromEntries(formData);
    
    // Mostrar mensaje de confirmación
    showNotification('¡Formulario enviado exitosamente! Redirigiendo a WhatsApp...', 'success');
    
    // Limpiar el formulario
    event.target.reset();
    
    // Abrir WhatsApp con los datos
    setTimeout(() => {
        sendWhatsAppMessage(data);
    }, 1000);
    
    // En una implementación real, aquí se enviaría la información a un servidor
    console.log('Datos del formulario:', data);
}

// Función para alternar el acordeón
function toggleAccordion(id) {
    const content = document.getElementById(id + '-content');
    const icon = document.getElementById(id + '-icon');
    
    if (content.classList.contains('active')) {
        content.classList.remove('active');
        icon.style.transform = 'rotate(0deg)';
    } else {
        // Cerrar todos los demás acordeones
        document.querySelectorAll('.accordion-content').forEach(item => {
            item.classList.remove('active');
        });
        document.querySelectorAll('[id$="-icon"]').forEach(item => {
            item.style.transform = 'rotate(0deg)';
        });
        
        // Abrir el acordeón actual
        content.classList.add('active');
        icon.style.transform = 'rotate(180deg)';
    }
}

// Funciones del modo administrativo
function toggleAdminMode() {
    isAdminMode = !isAdminMode;
    const body = document.body;
    const adminPanel = document.getElementById('adminPanel');
    
    if (isAdminMode) {
        body.classList.add('admin-active');
        adminPanel.classList.add('active');
    } else {
        body.classList.remove('admin-active');
        adminPanel.classList.remove('active');
    }
}

// Función para manejar la carga de imágenes en modo administrativo
function adminUploadImage(type) {
    if (!isAdminMode) return;
    
    const fileInput = document.getElementById(type + 'FileInput');
    if (fileInput) {
        fileInput.click();
    }
}

// Función para manejar la carga de archivos
function handleAdminImageUpload(event, type) {
    const file = event.target.files[0];
    if (!file) return;
    
    const reader = new FileReader();
    reader.onload = function(e) {
        let imageElement;
        
        switch(type) {
            case 'carolina':
                imageElement = document.getElementById('carolinaProfileImage');
                break;
            case 'testimonial1':
                imageElement = document.getElementById('testimonial1Image');
                break;
            case 'testimonial2':
                imageElement = document.getElementById('testimonial2Image');
                break;
            case 'testimonial3':
                imageElement = document.getElementById('testimonial3Image');
                break;
        }
        
        if (imageElement) {
            imageElement.src = e.target.result;
            showNotification('Imagen actualizada correctamente', 'success');
        }
    };
    
    reader.readAsDataURL(file);
}

// Función para guardar cambios (simulado)
function saveChanges() {
    saveAdminSettings();
    showNotification('Cambios guardados exitosamente', 'success');
}

// Función para mostrar notificaciones
function showNotification(message, type = 'info') {
    // Crear elemento de notificación
    const notification = document.createElement('div');
    notification.className = `fixed top-20 right-4 z-50 p-4 rounded-lg shadow-lg max-w-sm transform translate-x-full transition-transform duration-300 ${
        type === 'success' ? 'bg-green-500 text-white' : 
        type === 'error' ? 'bg-red-500 text-white' : 
        'bg-blue-500 text-white'
    }`;
    notification.textContent = message;
    
    // Agregar al DOM
    document.body.appendChild(notification);
    
    // Mostrar con animación
    setTimeout(() => {
        notification.style.transform = 'translateX(0)';
    }, 100);
    
    // Ocultar después de 3 segundos
    setTimeout(() => {
        notification.style.transform = 'translateX(100%)';
        setTimeout(() => {
            document.body.removeChild(notification);
        }, 300);
    }, 3000);
}

// Función para cargar configuración del administrador
function loadAdminSettings() {
    const savedImages = localStorage.getItem('yanbalAdminImages');
    if (savedImages) {
        try {
            const images = JSON.parse(savedImages);
            Object.keys(images).forEach(key => {
                const imgElement = document.getElementById(key + 'Image');
                if (imgElement && images[key]) {
                    imgElement.src = images[key];
                }
            });
        } catch (e) {
            console.log('No hay configuración guardada');
        }
    }
}

// Función para guardar configuración del administrador
function saveAdminSettings() {
    const images = {};
    
    const imageTypes = ['carolina', 'testimonial1', 'testimonial2', 'testimonial3'];
    imageTypes.forEach(type => {
        const imgElement = document.getElementById(type + 'Image');
        if (imgElement) {
            images[type] = imgElement.src;
        }
    });
    
    localStorage.setItem('yanbalAdminImages', JSON.stringify(images));
}

// Event listener para el scroll suave en los enlaces de navegación
document.addEventListener('click', function(e) {
    if (e.target.matches('a[href^="#"]')) {
        e.preventDefault();
        const targetId = e.target.getAttribute('href').substring(1);
        const targetElement = document.getElementById(targetId);
        if (targetElement) {
            targetElement.scrollIntoView({
                behavior: 'smooth',
                block: 'start'
            });
        }
    }
});

// Event listener para el cambio de tamaño de ventana
window.addEventListener('resize', function() {
    if (testimonialSwiper) {
        testimonialSwiper.update();
    }
});

// Event listener para el scroll (animaciones al hacer scroll)
window.addEventListener('scroll', function() {
    const navbar = document.querySelector('nav');
    if (window.scrollY > 100) {
        navbar.classList.add('shadow-lg');
    } else {
        navbar.classList.remove('shadow-lg');
    }
});

// Función para manejar el envío de WhatsApp con los datos del formulario
function sendWhatsAppMessage(data) {
    const message = `Hola Carolina, vi tu página y quiero agendar mi spa facial gratis.

Datos de contacto:
Nombre: ${data.nombre}
Teléfono: ${data.telefono}
Barrio/Zona: ${data.barrio}
Horario preferido: ${data.horario}
Comentarios: ${data.comentarios || 'Sin comentarios'}`;
    
    const encodedMessage = encodeURIComponent(message);
    const whatsappUrl = `https://wa.me/573144198482?text=${encodedMessage}`;
    
    window.open(whatsappUrl, '_blank');
}

// Función adicional para mejorar la experiencia de usuario
function addHoverEffects() {
    const buttons = document.querySelectorAll('button, .btn-primary');
    buttons.forEach(button => {
        button.addEventListener('mouseenter', function() {
            this.style.transform = 'translateY(-2px)';
        });
        
        button.addEventListener('mouseleave', function() {
            this.style.transform = 'translateY(0)';
        });
    });
}

// Inicializar efectos hover adicionales
document.addEventListener('DOMContentLoaded', addHoverEffects);
