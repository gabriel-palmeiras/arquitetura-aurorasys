# arquitetura-aurorasys

Título: Refatoração e Escalonamento de um SaaS Multi-Tenant para a Área da Saúde
Papel: Desenvolvedor Principal (Refatoração de Código e Escala Arquitetural)

1. O Desafio e Contexto de Negócio
O AuroraSys nasceu de uma necessidade real: otimizar e digitalizar os processos do consultório terapêutico da minha própria irmã. O que começou como uma solução desenhada sob medida provou seu valor operacional no dia a dia da clínica, e o desafio seguinte foi escalar essa operação para o mercado. Assumi a base de código para reescrever a arquitetura e transformar uma aplicação de uso único em um SaaS B2B capaz de atender múltiplos clientes (Multi-Tenant). O sistema precisava seguir regras rigorosas de privacidade (LGPD), garantindo o isolamento total para que não houvesse vazamento de dados entre clínicas diferentes. Hoje, a plataforma suporta simultaneamente terapeutas independentes e a complexa hierarquia de uma grande clínica.

2. Decisões Arquiteturais e Stack Técnica
Para suportar o processamento de agendas simultâneas e faturamento de convênios sem qualquer lentidão na interface, a aplicação foi isolada em frentes lógicas:

Frontend Seguro e Rápido: Construído com React 18 e Vite, utilizando tipagem rigorosa de ponta a ponta com TypeScript e Zod para evitar dados corrompidos.

Gestão de Estado Assíncrona: Substituição de gerenciadores de estado tradicionais pelo @tanstack/react-query. O cache de dados atua como a única fonte da verdade do sistema, garantindo que calendários e fluxos financeiros nunca fiquem dessincronizados do servidor.

Controle de Acesso Defensivo: Roteamento com interceptadores que avaliam a autenticação do usuário na origem, bloqueando o acesso a telas indevidas antes mesmo do carregamento visual.

3. Engenharia de Banco de Dados e Segurança (Destaque do Projeto)
Em um produto de saúde, delegar a segurança apenas à interface visual é um risco crítico. A arquitetura migrou para um isolamento nativo direto no banco de dados:

Segurança em Nível de Linha (PostgreSQL RLS): Toda consulta de leitura ou gravação é interceptada pelo motor do banco de dados (via Supabase), validando agressivamente as políticas de acesso vinculadas ao ID da clínica pertencente àquele usuário.

Infraestrutura Imutável: Controle de estrutura do banco de dados feito estritamente via migrações SQL, garantindo uma auditoria limpa e uma camada onde o próprio banco rejeita operações indevidas, mesmo se a aplicação falhar em barrá-las.

Prevenção de Conflitos de Agendamento: Restrições semânticas diretas no banco de dados garantem que tentativas simultâneas de marcação no mesmo horário (concorrência) sejam barradas instantaneamente. Isso previne a sobreposição de agendas sem sobrecarregar o servidor com lógicas complexas.
