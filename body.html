const { Client, LocalAuth, MessageMedia } = require('whatsapp-web.js');
const qrcode = require('qrcode-terminal');
const fs = require('fs');
const xlsx = require('xlsx');
const moment = require('moment');

// Cria o cliente do WhatsApp com armazenamento local da sessão
const client = new Client({
    authStrategy: new LocalAuth({ clientId: "client-one" })
});

// Gera o QR Code para autenticação
client.on('qr', (qr) => {
    qrcode.generate(qr, { small: true });
    console.log('Escaneie o QR Code para autenticar.');
});

// Evento disparado quando o bot está pronto
client.on('ready', () => {
    console.log('Bot está pronto e autenticado!');
});

// Carregar o arquivo JSON
let opcoes;
fs.readFile('./respostas.json', (err, data) => {
    if (err) {
        console.error('Erro ao ler o arquivo JSON:', err);
        return;
    }
    opcoes = JSON.parse(data);
});

// Configuração de números autorizados e senha
const numerosAutorizados = ['5511984179472']; // Números autorizados no formato internacional (sem +)
const senha = 'Inovare'; // Defina a senha

// Armazenamento de mensagens processadas
let estatisticas = {
    mensagens: {}, // Agora é um objeto para evitar duplicatas
    usuarios: {}, // Armazena a última interação por usuário para gerenciar inatividade
    ultimoEnvio: {}, // Inicializar a propriedade de último envio
    pesquisaEnviada: {} // Armazena se a pesquisa já foi enviada
};

// Tempo de inatividade para envio de mensagem de pesquisa
const TEMPO_INATIVIDADE_MS = 12 * 60 * 60 * 1000; // 12 horas

// Função para enviar mensagem de inatividade
function checarInatividade() {
    const agora = Date.now();
    for (const [usuario, ultimoTempo] of Object.entries(estatisticas.usuarios)) {
        if (agora - ultimoTempo >= TEMPO_INATIVIDADE_MS) {
            // Verifica se a pesquisa já foi enviada
            if (!estatisticas.pesquisaEnviada[usuario]) {
                client.sendMessage(usuario, 
                    'Sua opinião é muito importante para nós! 💬\n\n Gostaríamos de saber como foi a sua experiência. A sua resposta nos ajuda a melhorar cada vez mais!\n\nPor favor, dedique alguns minutos para preencher nossa pesquisa. O seu feedback é essencial para continuarmos oferecendo o melhor serviço possível.\n\n 🔗 https://forms.office.com/r/P57DBK0RzW');
                estatisticas.pesquisaEnviada[usuario] = true; // Marca como enviada
            }
        }
    }
}

// Verificar inatividade a cada 5 minutos
setInterval(checarInatividade, 12 * 60 * 60 * 1000); // 12 horas

// Mensagem recebida
client.on('message', async message => {
    const body = message.body.trim().toLowerCase(); // Remove espaços e converte para minúsculas
    const remetente = message.from;

    // Inicializa o último envio para o remetente se não existir
    if (!estatisticas.ultimoEnvio[remetente]) {
        estatisticas.ultimoEnvio[remetente] = Date.now();
    }

    // Atualiza a última interação do usuário
    estatisticas.usuarios[remetente] = Date.now();

    // Atualiza o timestamp de último envio para o remetente
    estatisticas.ultimoEnvio[remetente] = Date.now();

    // Salva ou atualiza os detalhes da mensagem
    const chave = '${remetente}-${body}';
    if (estatisticas.mensagens[chave]) {
        estatisticas.mensagens[chave].quantidade++;
        estatisticas.mensagens[chave].ultimaData = new Date().toISOString();
    } else {
        estatisticas.mensagens[chave] = {
            remetente,
            mensagem: body,
            quantidade: 1,
            primeiraData: new Date().toISOString(),
            ultimaData: new Date().toISOString()
        };
    }

    // Lista de saudações para verificar
    const saudacoes = ['bom dia', 'oi', 'olá', 'ola', 'oie', 'oii', 'e aí', 'boa tarde', 'boa noite', 'bom diaa', 'boom dia', 'booa tarde', 'boaa tarde', 'boa tardee','booa noite', 'boaa noite','boa noitee'];

    // Verificar se a mensagem é uma saudação
    if (saudacoes.some(saudacao => body.includes(saudacao)) && opcoes['saudacao']) {
        client.sendMessage(remetente, opcoes['saudacao']); // Envia a resposta de saudação
        return;
    }

    // Verificar se a mensagem é um número de opção
    if (body in opcoes) {
        client.sendMessage(remetente, opcoes[body]); // Envia a resposta sem marcar
        return;
    }

    // Verificar se a senha foi enviada por um número autorizado
    if (body === senha && numerosAutorizados.includes(remetente.replace('@c.us', ''))) {
        gerarPlanilhaEstatisticas();
        client.sendMessage(remetente, '📊 Planilha de estatísticas gerada com sucesso!');

        // Envio da planilha
        const caminhoArquivo = './estatisticas_mensagens.xlsx';
        const media = MessageMedia.fromFilePath(caminhoArquivo); // Cria o objeto de mídia
        await client.sendMessage(remetente, media); // Envia o arquivo
        return;
    }

    // Não responde se a senha for incorreta ou enviada por alguém não autorizado
    if (body === senha) {
        return;
    }

    // Mensagem padrão para opções inválidas
    //if (!saudacoes.some(saudacao => body.includes(saudacao)) && !(body in opcoes)) {
        //client.sendMessage(remetente, 'Desculpe, sou um robô e não consigo entender algumas palavras, tenho apenas respostas para os itens do menu inicial, (para visualizar o menu inicial digite "Menu")');
    //}
});

// Gera e salva a planilha Excel com as estatísticas
function gerarPlanilhaEstatisticas() {
    const data = [];

    // Processar estatísticas em formato de tabela
    for (const chave in estatisticas.mensagens) {
        const { remetente, mensagem, quantidade, primeiraData, ultimaData } = estatisticas.mensagens[chave];
        data.push({
            Remetente: remetente,
            Mensagem: mensagem,
            Quantidade: quantidade,
            'Primeira Data': moment(primeiraData).format('YYYY-MM-DD HH:mm:ss'),
            'Última Data': moment(ultimaData).format('YYYY-MM-DD HH:mm:ss')
        });
    }

    const workbook = xlsx.utils.book_new();
    const worksheet = xlsx.utils.json_to_sheet(data);

    xlsx.utils.book_append_sheet(workbook, worksheet, 'Estatísticas');

    const caminhoArquivo = './estatisticas_mensagens.xlsx';
    xlsx.writeFile(workbook, caminhoArquivo);

    console.log('Planilha salva em: ${caminhoArquivo}');
}

// Inicializar o cliente
client.initialize();
