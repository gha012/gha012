javascript
const { Client, GatewayIntentBits } = require('discord.js');

const client = new Client({ intents: [GatewayIntentBits.Guilds, GatewayIntentBits.GuildMessages] });

client.once('ready', () => {
    console.log(`تم تسجيل دخول البوت كـ ${client.user.tag}`);
});

client.on('messageCreate', message => {
    // بدء لعبة بومب
    if (message.content === '!بومب') {
        message.channel.send('💣 لعبة بومب بدأت! اكتب "انفجر" لتفجير القنبلة.');

        let chances = 3;
        const filter = response => response.content.toLowerCase() === 'انفجر' && response.author.id !== client.user.id;

        const collector = message.channel.createMessageCollector({ filter, time: 30000 });

        collector.on('collect', response => {
            if (chances > 0) {
                chances--;
                const explosion = Math.random() < 0.3; // 30% فرصة للانفجار
                if (explosion) {
                    message.channel.send(`${response.author} 💥 انفجرت القنبلة! أعد المحاولة!`);
                    collector.stop(); // إنهاء اللعبة
                } else {
                    message.channel.send(`${response.author} ✅ لقد نجوت! تبقى لديك ${chances} فرص.`);
                }
            } else {
                message.channel.send('⚠️ لم يتبق لديك فرص! اللعبة انتهت.');
                collector.stop(); // إنهاء اللعبة
            }
        });

        collector.on('end', collected => {
            if (chances === 0) {
                message.channel.send('🏁 اللعبة انتهت! شكراً للعب!');
            } else {
                message.channel.send('⏰ انتهت مدة اللعبة!');
            }
        });
    }
});

client.login(MTM1Nzg0NDEzMTc1MzQ5MjU4MA.GqsIef.Jh7TV_3YigmTx5opCjBtiN7RbGvjH0dJzAu9vY);
