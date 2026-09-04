[musa_plus_prototipo_24.html](https://github.com/user-attachments/files/31817118/musa_plus_prototipo_24.html)
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>MUSA+, Protótipo</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@500;600;700&family=Inter:wght@400;500;600&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/tabler-icons/2.44.0/iconfont/tabler-icons.min.css">
<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
<style>
  :root{
    --bg: #0C0C0D;
    --card: #171717;
    --card-2: #1E1E1E;
    --gold: #E8C58A;
    --gold-soft: #F4D9A5;
    --gold-deep: #4C3E25;
    --text: #FFFFFF;
    --text-dim: #B8B8B8;
    --text-faint: #8D8D8D;
    --border: rgba(255,255,255,0.06);
    --border-strong: rgba(232,197,138,0.35);
    --success: #8FAE7D;
    --success-soft: rgba(143,174,125,0.18);
  }
  *{box-sizing:border-box;}
  body{
    margin:0;
    background:#000;
    font-family:'Inter',sans-serif;
    color:var(--text);
    display:flex;
    justify-content:center;
    padding:40px 16px;
    min-height:100vh;
  }
  .phone{
    width:390px;
    max-width:100%;
    background:var(--bg);
    border-radius:36px;
    border:1px solid rgba(217,139,46,0.25);
    box-shadow:0 0 0 8px #050505, 0 30px 80px rgba(0,0,0,0.6), 0 0 60px rgba(217,139,46,0.05);
    overflow:hidden;
    position:relative;
  }
  .notch{height:28px;display:flex;align-items:center;justify-content:center;}
  .notch::before{content:'';width:100px;height:14px;background:#000;border-radius:10px;}
  .screen{height:800px;padding:0;position:relative;display:flex;flex-direction:column;}
  .screen-conteudo{flex:1;overflow-y:auto;padding:0 20px 30px;}
  .screen-conteudo::-webkit-scrollbar{display:none;}
  .view{display:none;}
  .view.active{display:block;animation:fadeIn .35s ease;}
  .fade-content{animation:fadeIn .3s ease;}
  @keyframes fadeIn{from{opacity:0;transform:translateY(6px);}to{opacity:1;transform:translateY(0);}}
  @keyframes introDnaFade{from{opacity:0;}to{opacity:1;}}
  @keyframes introDnaDraw{to{stroke-dashoffset:0;}}
  @keyframes introMusaFade{from{opacity:0;transform:translateY(8px);}to{opacity:1;transform:translateY(0);}}

  .backbar{display:flex;align-items:center;gap:8px;padding:14px 20px 12px;cursor:pointer;color:var(--gold-soft);font-size:13px;flex-shrink:0;border-bottom:1px solid var(--border);}

  .top-logo-fixa{display:flex;align-items:center;justify-content:center;padding:14px 0 10px;flex-shrink:0;border-bottom:1px solid var(--border);}
  .top-logo-fixa span{font-family:'Playfair Display',serif;font-size:15px;font-weight:700;color:var(--gold);letter-spacing:0.5px;}

  .bottom-nav-fixa{display:flex;flex-shrink:0;border-top:1px solid var(--border);background:var(--card);}
  .bottom-nav-item{flex:1;display:flex;flex-direction:column;align-items:center;gap:3px;padding:10px 4px 8px;cursor:pointer;}
  .bottom-nav-item .nav-icon-svg{fill:var(--text-faint);}
  .bottom-nav-item span{font-size:9.5px;color:var(--text-faint);}
  .bottom-nav-item.ativo .nav-icon-svg, .bottom-nav-item.ativo span{fill:var(--gold-soft);color:var(--gold-soft);}
  .backbar i{font-size:16px;}

  /* ===== LAYOUT DESKTOP DO PERSONAL (sidebar), só entra em telas largas ===== */
  .sidebar-personal{ display:none; }
  .sidebar-personal .side-logo{ font-family:'Playfair Display',serif; font-size:17px; font-weight:600; color:var(--gold-soft); padding:6px 10px 22px; letter-spacing:0.5px; }
  .sidebar-personal .side-grupo-label{ font-size:10px; text-transform:uppercase; letter-spacing:0.6px; color:var(--text-faint); padding:14px 10px 6px; }
  .sidebar-personal .side-item{ display:flex; align-items:center; gap:10px; padding:10px 10px; border-radius:10px; cursor:pointer; font-size:13px; color:var(--text-dim); margin-bottom:2px; }
  .sidebar-personal .side-item i{ font-size:16px; width:18px; text-align:center; color:var(--text-faint); }
  .sidebar-personal .side-item:hover{ background:var(--card-2); }
  .sidebar-personal .side-item.ativo{ background:var(--gold-deep); color:var(--gold-soft); }
  .sidebar-personal .side-item.ativo i{ color:var(--gold-soft); }
  .sidebar-personal .side-sair{ margin-top:auto; }

  .modal-overlay-avisos{ display:none; position:fixed; inset:0; background:rgba(0,0,0,0.72); z-index:200; align-items:center; justify-content:center; padding:24px; }
  .modal-overlay-avisos .modal-card{ background:var(--card); border:1px solid var(--border-strong); border-radius:18px; padding:24px 20px; max-width:340px; width:100%; text-align:center; }
  .modal-overlay-avisos .modal-card h3{ font-family:'Playfair Display',serif; font-size:18px; color:var(--gold-soft); margin:0 0 10px; }
  .modal-overlay-avisos .modal-card p{ font-size:13px; color:var(--text-dim); margin:0 0 20px; line-height:1.5; }
  .modal-overlay-avisos .modal-botoes{ display:flex; gap:10px; }
  .modal-overlay-avisos .modal-botoes button{ flex:1; border-radius:12px; padding:11px; font-size:13px; font-weight:600; cursor:pointer; border:1px solid var(--border); background:var(--card-2); color:var(--text-dim); font-family:inherit; }
  .modal-overlay-avisos .modal-botoes button.btn-aceitar{ background:linear-gradient(135deg,var(--gold-soft),var(--gold)); color:#1A1409; border:none; }

  @media (min-width: 1024px){
    body{ padding:28px; align-items:flex-start; }
    .phone.modo-personal{
      width:100%; max-width:1240px; border-radius:16px;
      box-shadow:0 20px 60px rgba(0,0,0,0.5);
      display:flex; flex-direction:row; align-items:stretch;
    }
    .phone.modo-personal .notch{ display:none; }
    .phone.modo-personal .sidebar-personal{
      display:flex; flex-direction:column; width:230px; flex-shrink:0;
      background:var(--card); border-right:1px solid var(--border);
      padding:20px 12px; height:calc(100vh - 56px); max-height:900px; overflow-y:auto;
    }
    .phone.modo-personal .screen{ flex:1; height:calc(100vh - 56px); max-height:900px; min-width:0; }
    .phone.modo-personal .screen-conteudo{ padding:28px 44px 60px; max-width:1000px; }
    .phone.modo-personal .backbar{ display:none !important; }
    .phone.modo-personal .top-logo-fixa{ display:none !important; }
    .phone.modo-personal #alunas-list{ display:grid; grid-template-columns:repeat(2,1fr); gap:10px; align-content:start; }
    .phone.modo-personal .stat-grid{ grid-template-columns:repeat(4,1fr); }
    .phone.modo-personal #label-ferramentas-dashboard,
    .phone.modo-personal #grid-ferramentas-personal{ display:none; }
    .phone.modo-personal #ex-lista{ display:grid !important; grid-template-columns:repeat(2,1fr); gap:8px; }
  }

  /* LAUNCHER */
  .launcher{padding-top:56px;text-align:center;}
  .brand{
    font-family:'Playfair Display',serif;font-size:32px;font-weight:600;letter-spacing:1px;
    background:linear-gradient(135deg,#F4D9A5,#E8C58A 45%,#4C3E25);
    -webkit-background-clip:text;background-clip:text;color:transparent;margin:0;
  }
  .brand-sub{font-size:11px;letter-spacing:2.5px;color:var(--text-faint);text-transform:uppercase;margin:4px 0 0;}
  .sub{color:var(--text-dim);font-size:13px;margin:22px 0 40px;letter-spacing:0.3px;}
  .profiles{display:flex;gap:18px;justify-content:center;flex-wrap:wrap;}
  .estrela-launcher{position:relative;width:260px;height:260px;}
  .satelite{position:absolute;width:76px;text-align:center;cursor:pointer;}
  .satelite .avatar{width:56px;height:56px;margin:0 auto 4px;background:var(--card) !important;}
  .satelite .profile-name{font-size:9.5px;color:var(--text-dim);}
  .satelite-topo{top:0;left:50%;transform:translateX(-50%);}
  .satelite-esquerda{top:50%;left:0;transform:translateY(-50%);}
  .satelite-direita{top:50%;right:0;transform:translateY(-50%);}
  .satelite-baixo{bottom:0;left:50%;transform:translateX(-50%);}
  .satelite-centro{position:absolute;top:50%;left:50%;transform:translate(-50%,-50%);width:100px;text-align:center;cursor:pointer;}
  .profile-card{cursor:pointer;text-align:center;transition:transform .25s ease;width:100px;}
  .profile-card:hover{transform:translateY(-3px);}
  .avatar{width:78px;height:78px;border-radius:22px;display:flex;align-items:center;justify-content:center;margin:0 auto 10px;position:relative;}
  .avatar.gold{background:radial-gradient(circle at 30% 25%, #F4D9A5, #E8C58A 55%, #4C3E25 100%);box-shadow:0 8px 26px rgba(217,139,46,0.35);}
  .avatar.gold span{font-family:'Playfair Display',serif;font-size:30px;font-weight:600;color:#0B0B0C;}
  .avatar.dark{background:var(--card-2);border:1px solid var(--border);}
  .avatar.dark i{font-size:26px;color:var(--gold-soft);}
  .profile-name{font-size:12px;font-weight:600;margin:0;line-height:1.3;}
  .profile-desc{font-size:11px;color:var(--text-dim);margin:3px 0 0;line-height:1.3;}

  h1.page-title{font-family:'Playfair Display',serif;font-size:22px;font-weight:600;margin:18px 0 4px;}
  .page-sub{font-size:12px;color:var(--text-dim);margin:0 0 18px;}
  .section-label{font-size:12px;font-weight:600;color:var(--gold-soft);text-transform:uppercase;letter-spacing:0.6px;margin:0 0 10px;}

  .hero-card{background:linear-gradient(135deg, #241E14, #16130D);border:1px solid var(--border);border-radius:18px;padding:20px;margin-bottom:16px;cursor:pointer;position:relative;overflow:hidden;}
  .hero-card::after{content:'';position:absolute;top:-40%;right:-20%;width:220px;height:220px;background:radial-gradient(circle, rgba(217,139,46,0.18), transparent 70%);}
  .hero-eyebrow{font-size:11px;letter-spacing:1px;text-transform:uppercase;color:var(--gold-soft);margin:0 0 8px;position:relative;}
  .hero-title{font-family:'Playfair Display',serif;font-size:19px;font-weight:600;margin:0 0 8px;position:relative;}
  .hero-cta{font-size:12px;color:var(--text-dim);margin:0;position:relative;}

  .pending-card{background:rgba(217,139,46,0.08);border:1px solid var(--border-strong);border-radius:14px;padding:12px 14px;margin-bottom:26px;cursor:pointer;display:flex;justify-content:space-between;align-items:center;gap:10px;}
  .pending-card p{margin:0;font-size:12px;color:#EDE0BE;}
  .pending-card i{color:var(--gold-soft);font-size:16px;flex-shrink:0;}

  .row{display:flex;gap:12px;overflow-x:auto;margin-bottom:26px;padding-bottom:4px;scrollbar-width:none;}
  .row::-webkit-scrollbar{display:none;}
  .poster{min-width:128px;background:var(--card);border:1px solid var(--border);border-radius:14px;padding:12px;cursor:pointer;position:relative;transition:border-color .2s ease, transform .2s ease;}
  .poster:hover{border-color:rgba(217,139,46,0.5);transform:translateY(-2px);}
  .poster.pendente{border-color:var(--border-strong);}
  .poster .check{position:absolute;top:10px;right:10px;color:var(--gold-soft);font-size:13px;}
  .poster .pend-tag{position:absolute;top:9px;right:9px;font-size:11px;color:var(--gold-soft);background:rgba(217,139,46,0.15);padding:2px 6px;border-radius:6px;}
  .poster .sub{font-size:11px;color:var(--text-faint);margin:0;}
  .poster .title{font-size:12px;font-weight:600;margin:5px 0 0;color:var(--text);}

  .stat-grid{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:26px;}

  .dna-score-card{display:flex;align-items:center;gap:14px;background:radial-gradient(90% 100% at 12% -5%, rgba(232,197,138,0.22), rgba(232,197,138,0.05) 35%, transparent 60%), #120F0D;border:1px solid var(--border-strong);border-radius:22px;padding:22px 20px;cursor:pointer;box-shadow:0 12px 30px rgba(0,0,0,0.35), inset 0 1px 0 rgba(255,255,255,0.04);opacity:0;animation:fadeSlideUp .5s ease forwards;}
  .dna-score-helix{width:52px;height:52px;border-radius:50%;background:radial-gradient(circle at 30% 25%, rgba(232,197,138,0.16), var(--card-2) 70%);border:1px solid var(--border-strong);display:flex;align-items:center;justify-content:center;flex-shrink:0;box-shadow:0 0 18px rgba(232,197,138,0.08);}
  .dna-score-helix i{font-size:22px;color:var(--gold-soft);}
  .dna-score-left{flex:1;}
  .dna-score-label{font-size:10px;letter-spacing:1.5px;color:var(--text-faint);text-transform:uppercase;margin:0 0 6px;}
  .dna-score-numero{font-family:'Playfair Display',serif;font-size:48px;font-weight:700;color:var(--gold-soft);margin:0;line-height:1;}
  .dna-score-comparativo{font-size:12.5px;color:var(--success);margin:6px 0 0;font-weight:600;}
  .dna-score-ring{position:relative;width:76px;height:76px;flex-shrink:0;display:flex;align-items:center;justify-content:center;}
  .dna-score-ring-valor{position:absolute;font-size:15px;font-weight:700;color:var(--text);}
  .dna-score-ver-detalhes{display:inline-flex;align-items:center;gap:4px;font-size:11px;color:var(--gold-soft);margin-top:10px;font-weight:600;}

  .indicadores-grid{display:grid;grid-template-columns:repeat(6,1fr);gap:10px;margin-top:14px;}
  .indicador-card{grid-column:span 2;background:radial-gradient(140% 100% at 20% 0%, rgba(232,197,138,0.08), transparent 45%), #151210;border:1px solid var(--border);border-radius:18px;padding:13px;box-shadow:0 6px 16px rgba(0,0,0,0.25);opacity:0;animation:fadeSlideUp .5s ease forwards;}
  .indicador-card.largo{grid-column:span 3;}
  .indicador-icone{width:28px;height:28px;border-radius:9px;background:var(--card-2);display:flex;align-items:center;justify-content:center;margin-bottom:7px;}
  .indicador-icone i{font-size:13px;color:var(--gold-soft);}
  .indicador-label{font-size:11.5px;color:var(--text-dim);margin:0 0 3px;}
  .indicador-valor{font-size:20px;font-weight:700;color:var(--text);margin:0 0 7px;}
  .indicador-barra-bg{height:3px;border-radius:100px;background:var(--border);overflow:hidden;margin-bottom:6px;}
  .indicador-barra-fill{height:100%;border-radius:100px;background:linear-gradient(90deg,#E8C58A,#F4D9A5);}
  .indicador-caption{font-size:10.5px;color:var(--text-faint);margin:0;}
  .stat-card{background:var(--card);border:1px solid var(--border);border-radius:14px;padding:14px;}

  @keyframes fadeSlideUp{from{opacity:0;transform:translateY(10px);}to{opacity:1;transform:translateY(0);}}
  .stat-label{font-size:11px;color:var(--text-dim);margin:0;}
  .stat-value{font-family:'Playfair Display',serif;font-size:20px;font-weight:600;margin:4px 0 0;}
  .stat-meta{font-size:11px;color:var(--text-faint);margin:4px 0 0;}
  .stat-meta.gold{color:var(--gold-soft);}

  .insight{background:linear-gradient(135deg, rgba(217,139,46,0.12), rgba(217,139,46,0.03));border:1px solid var(--border);border-radius:14px;padding:12px 14px;margin-bottom:18px;}
  .insight p{font-size:12px;margin:0;color:#EDE0BE;}

  .ring-wrap{display:flex;align-items:center;gap:18px;margin-bottom:20px;}
  .ring-num{font-family:'Playfair Display',serif;font-size:26px;font-weight:600;margin:0;}
  .ring-label{font-size:12px;color:var(--text-dim);margin:2px 0 0;}

  .video-block{background:var(--card);border:1px solid var(--border);border-radius:16px;height:190px;display:flex;align-items:center;justify-content:center;margin-bottom:16px;position:relative;overflow:hidden;}
  .video-block iframe{width:100%;height:100%;position:relative;z-index:1;}
  .yt-fallback{display:flex;align-items:center;gap:5px;font-size:12px;color:var(--text-dim);text-decoration:none;margin:-8px 0 16px;}
  .yt-fallback:hover{color:var(--gold-soft);}
  .video-block::after{content:'';position:absolute;inset:0;background:radial-gradient(circle at 50% 50%, rgba(217,139,46,0.08), transparent 60%);}
  .video-block i{font-size:36px;color:var(--gold-soft);position:relative;}

  .list-item{background:var(--card);border:1px solid var(--border);border-radius:12px;padding:11px 14px;margin-bottom:8px;font-size:13px;display:flex;justify-content:space-between;align-items:center;}
  .list-item .tag{font-size:11px;color:var(--gold-soft);}

  .badge{display:inline-block;background:rgba(217,139,46,0.14);color:var(--gold-soft);font-size:11px;text-transform:uppercase;letter-spacing:0.5px;padding:4px 10px;border-radius:8px;margin-bottom:10px;}

  .info-box{background:var(--card-2);border:1px solid var(--border);border-radius:14px;padding:14px;margin-bottom:16px;}
  .info-box .lbl{font-size:11px;text-transform:uppercase;letter-spacing:0.5px;color:var(--gold-soft);margin:0 0 6px;}
  .card-item-title{font-family:'Playfair Display',serif;font-size:15px;font-weight:600;margin:0 0 6px;color:var(--text);}
  .info-box p.txt{font-size:13px;color:var(--text);margin:0 0 10px;line-height:1.5;}
  .info-box p.txt:last-child{margin-bottom:0;}

  .month-row{display:flex;justify-content:space-between;align-items:center;padding:10px 0;border-bottom:1px solid var(--border);}
  .month-row:last-child{border-bottom:none;}
  .month-name{font-size:13px;color:var(--text-dim);}
  .month-bar-wrap{flex:1;margin:0 12px;background:#221F18;border-radius:6px;height:8px;overflow:hidden;}
  .month-bar{height:100%;background:linear-gradient(90deg,#4C3E25,#E8C58A);}
  .month-val{font-size:12px;font-weight:600;min-width:44px;text-align:right;}

  .filter-row{display:flex;gap:8px;overflow-x:auto;margin-bottom:20px;padding-bottom:4px;scrollbar-width:none;}
  .filter-row::-webkit-scrollbar{display:none;}
  .chip{flex-shrink:0;background:var(--card);border:1px solid var(--border);border-radius:20px;padding:7px 14px;font-size:12px;color:var(--text-dim);cursor:pointer;white-space:nowrap;transition:all .2s ease;}
  @keyframes pulseGold{0%{transform:scale(1);box-shadow:0 0 0 rgba(217,139,46,0.7);}50%{transform:scale(1.12);box-shadow:0 0 16px rgba(217,139,46,0.55);}100%{transform:scale(1);box-shadow:0 0 0 rgba(217,139,46,0);}}
  .chip.pulse{animation:pulseGold .55s ease;}
  @keyframes toastIn{from{opacity:0;transform:translateY(8px);}to{opacity:1;transform:translateY(0);}}
  .toast-celebracao{animation:toastIn .3s ease;background:linear-gradient(135deg,#F4D9A5,#E8C58A 55%,#4C3E25);color:#0B0B0C;font-weight:600;font-size:12px;padding:10px 14px;border-radius:10px;text-align:center;margin-bottom:10px;}
  .chip.active{background:linear-gradient(135deg,#F4D9A5,#E8C58A 55%,#4C3E25);color:#0B0B0C;font-weight:600;border-color:transparent;}

  .poster-grid{display:grid;grid-template-columns:1fr 1fr;gap:14px;}
  .vposter{cursor:pointer;}
  .vcover{
    aspect-ratio:2/3;border-radius:14px;background:linear-gradient(150deg,#241E14,#171717);
    border:1px solid var(--border);display:flex;align-items:center;justify-content:center;
    position:relative;overflow:hidden;margin-bottom:8px;transition:border-color .2s ease, transform .2s ease;
  }
  .vposter:hover .vcover{border-color:var(--border-strong);transform:translateY(-2px);}
  .vcover::after{content:'';position:absolute;inset:0;background:radial-gradient(circle at 50% 40%, rgba(217,139,46,0.14), transparent 65%);}
  .vcover i.play{font-size:26px;color:var(--gold-soft);position:relative;}
  .vcover .lock-icon{position:absolute;top:10px;right:10px;width:26px;height:26px;border-radius:8px;background:rgba(0,0,0,0.55);display:flex;align-items:center;justify-content:center;}
  .vcover .lock-icon i{font-size:13px;color:var(--gold-soft);}
  .vtitle{font-size:12px;font-weight:600;text-align:center;margin:0;line-height:1.3;color:var(--text);}

  .ferramenta-compacta{background:linear-gradient(155deg,var(--card) 0%,var(--card-2) 100%);border:1px solid var(--border);border-radius:16px;padding:14px 10px;cursor:pointer;display:flex;flex-direction:column;align-items:center;gap:8px;transition:border-color .2s ease,transform .2s ease,box-shadow .2s ease;}
  .ferramenta-compacta:active{transform:scale(0.96);}
  .ferramenta-compacta:hover{border-color:var(--border-strong);box-shadow:0 4px 16px rgba(232,197,138,0.12);}
  .ferramenta-compacta i{color:var(--gold-soft);font-size:22px;}
  .ferramenta-compacta p{font-weight:500;}
  .ferramenta-compacta:hover{border-color:var(--border-strong);}
  .ferramenta-compacta i{font-size:20px;color:var(--gold-soft);}
  .ferramenta-compacta p{font-size:11px;font-weight:600;text-align:center;margin:0;line-height:1.25;color:var(--text);}

  /* PERSONAL */
  .avatar.dark.admin i{color:var(--text-dim);}
  .alert-row{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:24px;}
  .alert-stat{background:var(--card);border:1px solid var(--border);border-radius:14px;padding:12px;}
  .alert-stat .num{font-family:'Playfair Display',serif;font-size:20px;font-weight:600;margin:0;}
  .alert-stat .lbl2{font-size:11px;color:var(--text-dim);margin:2px 0 0;}

  .aluna-card{background:var(--card);border:1px solid var(--border);border-radius:14px;padding:13px 14px;margin-bottom:10px;cursor:pointer;display:flex;justify-content:space-between;align-items:center;gap:10px;transition:border-color .2s ease;}
  .aluna-card:hover{border-color:var(--border-strong);}
  .aluna-name{font-size:13px;font-weight:600;margin:0 0 4px;}
  .aluna-meta{font-size:11px;color:var(--text-faint);margin:0;}
  .status-dot{width:7px;height:7px;border-radius:50%;display:inline-block;margin-right:6px;}
  .status-dot.ok{background:var(--gold-soft);}
  .status-dot.alerta{background:#C9784A;}
  .status-dot.vencendo{background:#E2A33D;}
  .status-dot.lead{background:var(--text-faint);}
  .status-txt{font-size:11px;}
  .status-txt.ok{color:var(--gold-soft);}
  .status-txt.alerta{color:#C9784A;}
  .status-txt.vencendo{color:#E2A33D;}
  .status-txt.lead{color:var(--text-faint);}

  .tool-card{background:linear-gradient(135deg,#1E1E1E,#141310);border:1px solid var(--border);border-radius:14px;padding:16px;margin-bottom:12px;cursor:pointer;display:flex;align-items:center;gap:14px;transition:border-color .2s ease;}
  .tool-card:hover{border-color:var(--border-strong);}
  .tool-card i{font-size:22px;color:var(--gold-soft);}
  .tool-card .tool-title{font-size:13px;font-weight:600;margin:0;}
  .tool-card .tool-desc{font-size:11px;color:var(--text-dim);margin:2px 0 0;}

  .local-back{display:inline-flex;align-items:center;gap:6px;color:var(--gold-soft);font-size:12px;font-weight:500;cursor:pointer;margin-bottom:14px;background:var(--card-2);border:1px solid var(--border);border-radius:100px;padding:7px 14px 7px 10px;transition:border-color .2s ease;}
  .local-back:hover{border-color:var(--border-strong);}
  .local-back i{font-size:14px;}

  .acao-pill{background:var(--card-2);border:1px solid var(--border-strong);border-radius:100px;padding:6px 13px;font-size:11px;font-weight:500;color:var(--gold-soft);cursor:pointer;transition:all .15s ease;}
  .acao-pill:hover{background:var(--gold-deep);}
  .acao-pill.destrutiva{border-color:rgba(226,163,61,0.4);color:#E2A33D;}
  .acao-pill.destrutiva:hover{background:rgba(226,163,61,0.12);}

  .chip-list{display:flex;flex-wrap:wrap;gap:6px;margin-bottom:4px;}
  .desvio-chip{font-size:11px;background:rgba(217,139,46,0.12);color:var(--gold-soft);padding:5px 10px;border-radius:8px;border:1px solid var(--border);cursor:pointer;}
  .desvio-chip.selected{background:linear-gradient(135deg,#F4D9A5,#E8C58A 55%,#4C3E25);color:#0B0B0C;font-weight:600;border-color:transparent;}

  .form-group{margin-bottom:12px;}
  .form-label{font-size:11px;color:var(--text-dim);margin:0 0 5px;display:block;}
  .form-input, .form-select{width:100%;background:var(--card-2);border:1px solid var(--border);border-radius:10px;padding:9px 11px;font-size:13px;color:var(--text);font-family:'Inter',sans-serif;}
  .form-input:focus, .form-select:focus{outline:none;border-color:var(--border-strong);}
  .btn-gold{width:100%;background:linear-gradient(135deg,#F4D9A5,#E8C58A 55%,#4C3E25);color:#0B0B0C;font-weight:600;font-size:13px;border:none;border-radius:10px;padding:11px;cursor:pointer;margin-top:6px;}
  .exercicio-item{background:var(--card);border:1px solid var(--border);border-radius:12px;padding:11px 13px;margin-bottom:8px;}
  .exercicio-item .ex-name{font-size:13px;font-weight:600;margin:0 0 3px;}
  .exercicio-item .ex-meta{font-size:11px;color:var(--text-faint);margin:0;}
  .exercicio-item .ex-video{font-size:11px;margin-top:4px;display:inline-block;}
  .exercicio-item .ex-video.pending{color:#E2A33D;}
  .exercicio-item .ex-video.ok{color:var(--gold-soft);}
</style>
</head>
<body>

<div id="cronometro-fullscreen-overlay" style="display:none;position:fixed;inset:0;background:#0C0C0D;z-index:9999;flex-direction:column;align-items:center;justify-content:center;">
  <div onclick="minimizarCronometro()" style="position:absolute;top:24px;right:24px;width:36px;height:36px;border-radius:50%;background:var(--card-2);display:flex;align-items:center;justify-content:center;cursor:pointer;">
    <i class="ti ti-minus" style="color:var(--gold-soft);font-size:20px;"></i>
  </div>
  <p id="cronometro-fullscreen-label" style="color:var(--text-faint);font-size:13px;letter-spacing:1px;text-transform:uppercase;margin-bottom:20px;">Descanso</p>
  <div style="position:relative;width:220px;height:220px;display:flex;align-items:center;justify-content:center;">
    <svg width="220" height="220" viewBox="0 0 220 220" style="position:absolute;top:0;left:0;transform:rotate(-90deg);">
      <circle cx="110" cy="110" r="98" fill="none" stroke="#26231C" stroke-width="8"/>
      <circle id="cronometro-fullscreen-ring" cx="110" cy="110" r="98" fill="none" stroke="url(#goldringcronometro)" stroke-width="8" stroke-linecap="round" stroke-dasharray="616" stroke-dashoffset="0"/>
      <defs><linearGradient id="goldringcronometro" x1="0" y1="0" x2="1" y2="1"><stop offset="0%" stop-color="#F4D9A5"/><stop offset="100%" stop-color="#E8C58A"/></linearGradient></defs>
    </svg>
    <p id="cronometro-fullscreen-numero" style="font-family:'Playfair Display',serif;font-size:44px;font-weight:600;color:var(--text-main);">60s</p>
  </div>
  <div style="display:flex;gap:16px;margin-top:32px;">
    <button onclick="pausarOuRetomarCronometro()" id="cronometro-fullscreen-btn" class="btn-gold" style="width:auto;padding:12px 28px;margin:0;">Pausar</button>
    <button onclick="pararCronometroFullscreen()" style="width:auto;padding:12px 28px;background:transparent;border:1px solid var(--border);color:var(--text-faint);border-radius:100px;font-size:14px;cursor:pointer;">Encerrar</button>
  </div>
</div>

<div id="banner-notificacao-simulada" style="display:none;position:fixed;top:16px;left:16px;right:16px;max-width:358px;margin:0 auto;background:var(--card-2);border:1px solid var(--border-strong);border-radius:16px;padding:14px;z-index:10000;box-shadow:0 10px 30px rgba(0,0,0,0.5);">
  <div style="display:flex;align-items:flex-start;gap:10px;">
    <div style="width:36px;height:36px;border-radius:10px;background:linear-gradient(135deg,#F4D9A5,#E8C58A);display:flex;align-items:center;justify-content:center;flex-shrink:0;font-weight:700;color:#1A1409;font-family:'Playfair Display',serif;">D</div>
    <div style="flex:1;">
      <p style="font-size:10px;color:var(--text-faint);margin:0 0 2px;text-transform:uppercase;letter-spacing:0.5px;">DNA MUSA · agora</p>
      <p id="banner-notif-titulo" style="font-size:13px;font-weight:600;margin:0 0 2px;"></p>
      <p id="banner-notif-corpo" style="font-size:12px;color:var(--text-dim);margin:0 0 10px;"></p>
      <div id="banner-notif-botoes" style="display:flex;gap:8px;">
        <span class="chip" style="cursor:pointer;flex:1;text-align:center;" onclick="responderCheckIn(true)">Sim, vou treinar</span>
        <span class="chip" style="cursor:pointer;flex:1;text-align:center;" onclick="responderCheckIn(false)">Hoje não</span>
      </div>
    </div>
  </div>
</div>

<div id="cronometro-pill-minimizado" style="display:none;position:fixed;bottom:90px;right:16px;background:var(--card-2);border:1px solid var(--border);border-radius:100px;padding:8px 14px;z-index:9998;cursor:pointer;align-items:center;gap:6px;" onclick="expandirCronometro()">
  <i class="ti ti-clock" style="color:var(--gold-soft);font-size:14px;"></i>
  <span id="cronometro-pill-numero" style="font-size:13px;font-weight:600;color:var(--gold-soft);">60s</span>
</div>

<div class="phone" id="phone-container">
  <div class="notch"></div>
  <div class="sidebar-personal" id="sidebar-personal"></div>
  <div class="modal-overlay-avisos" id="modal-aceitar-avisos">
    <div class="modal-card">
      <h3>Avisos do seu personal</h3>
      <p>Quer receber aqui no app avisos importantes do seu personal, tipo ajustes no treino e comunicados? Você pode mudar isso depois, quando quiser.</p>
      <div class="modal-botoes">
        <button onclick="responderAceiteAvisos(false)">Agora não</button>
        <button class="btn-aceitar" onclick="responderAceiteAvisos(true)">Aceitar</button>
      </div>
    </div>
  </div>
  <div class="modal-overlay-avisos" id="modal-lembrar-login">
    <div class="modal-card">
      <h3>Salvar esse login?</h3>
      <p>Quer que esse aparelho entre direto nessa conta da próxima vez que abrir o app? Se preferir escolher o login toda vez (bom quando o aparelho é compartilhado), pode dizer não.</p>
      <div class="modal-botoes">
        <button onclick="responderLembrarLogin(false)">Não salvar</button>
        <button class="btn-aceitar" onclick="responderLembrarLogin(true)">Salvar login</button>
      </div>
    </div>
  </div>
  <div class="screen">
    <p style="position:fixed;top:2px;left:0;right:0;text-align:center;font-size:9px;color:var(--text-faint);z-index:999999;letter-spacing:1px;pointer-events:none;">versão 2026-08-05-A</p>

    <div id="backbar" class="backbar" style="display:none;" onclick="goBack()">
      <i class="ti ti-arrow-left"></i>
      <span id="backlabel">Voltar</span>
    </div>

    <div id="top-logo-fixa" class="top-logo-fixa" style="display:none;">
      <span>DNA MUSA</span>
    </div>

    <div class="screen-conteudo">
    <!-- LOGIN -->
    <div class="view active" data-view="login">
      <div class="launcher">
        <p class="brand" style="opacity:0;animation:introMusaFade 1s ease forwards;">DNA MUSA</p>
        <p class="brand-sub" style="opacity:0;animation:introMusaFade 1s ease forwards .2s;">Team Fernandes</p>
        <p class="sub" style="margin-bottom:24px;">Entre pra continuar</p>

        <div class="filter-row" id="login-tipo-row" style="justify-content:center;margin-bottom:18px;">
          <div class="chip active" id="login-tab-aluna" onclick="alternarTipoLogin('aluna')">Sou aluna</div>
          <div class="chip" id="login-tab-personal" onclick="alternarTipoLogin('personal')">Sou o personal</div>
        </div>

        <div id="login-form-aluna">
          <div class="form-group"><input class="form-input" id="login-aluna-email" type="email" placeholder="Seu e-mail" autocomplete="username"></div>
          <div class="form-group"><input class="form-input" id="login-aluna-senha" type="password" placeholder="Crie ou digite sua senha" autocomplete="current-password"></div>
          <p class="page-sub" style="font-size:11px;margin-top:-6px;">Primeira vez? Só digitar e-mail e uma senha já cadastra você.</p>
          <button class="btn-gold" onclick="loginAluna()">Entrar</button>
          <p id="login-aluna-erro" style="color:#E2A33D;font-size:12px;text-align:center;display:none;"></p>
        </div>

        <div id="login-form-personal" style="display:none;">
          <div class="form-group"><input class="form-input" id="login-personal-email" type="email" placeholder="Seu e-mail" autocomplete="username"></div>
          <div class="form-group"><input class="form-input" id="login-personal-senha" type="password" placeholder="Sua senha de personal" autocomplete="current-password"></div>
          <button class="btn-gold" onclick="loginPersonal()">Entrar</button>
          <p id="login-personal-erro" style="color:#E2A33D;font-size:12px;text-align:center;display:none;"></p>
        </div>
      </div>
    </div>

    <!-- INTRO ANIMADA -->
    <div class="view" data-view="intro" onclick="pularIntro()" style="cursor:pointer;">
      <div class="launcher" style="justify-content:center;">
        <svg id="intro-dna" width="90" height="180" viewBox="0 0 90 180" style="margin:0 auto;opacity:0;animation:introDnaFade 1.8s ease forwards;">
          <path d="M15,5 C15,45 75,45 75,85 C75,125 15,125 15,165" fill="none" stroke="#E8C58A" stroke-width="1.5" stroke-dasharray="400" stroke-dashoffset="400" style="animation:introDnaDraw 1.6s ease forwards;"/>
          <path d="M75,5 C75,45 15,45 15,85 C15,125 75,125 75,165" fill="none" stroke="#F4D9A5" stroke-width="1.5" stroke-dasharray="400" stroke-dashoffset="400" style="animation:introDnaDraw 1.6s ease forwards;animation-delay:.15s;"/>
          <g stroke="#E8C58A" stroke-width="1" opacity="0.7">
            <line x1="18" y1="20" x2="72" y2="20"/><line x1="27" y1="45" x2="63" y2="45"/>
            <line x1="45" y1="65" x2="45" y2="65"/><line x1="27" y1="85" x2="63" y2="85"/>
            <line x1="18" y1="105" x2="72" y2="105"/><line x1="27" y1="125" x2="63" y2="125"/>
            <line x1="45" y1="145" x2="45" y2="145"/><line x1="27" y1="160" x2="63" y2="160"/>
          </g>
        </svg>
        <p id="intro-musa" class="brand" style="opacity:0;font-size:30px;letter-spacing:5px;font-weight:500;margin-top:18px;animation:introMusaFade 1.6s ease forwards;animation-delay:1.9s;">DNA MUSA</p>
        <p id="intro-sub" style="opacity:0;color:var(--gold-soft);font-size:12px;letter-spacing:3px;text-transform:uppercase;margin-top:6px;animation:introMusaFade 1.6s ease forwards;animation-delay:2.3s;">Team Fernandes</p>
      </div>
    </div>

    <!-- ONBOARDING -->
    <div class="view" data-view="onboarding">
      <div class="launcher">
        <p class="brand" style="opacity:0;animation:introMusaFade 1s ease forwards;">DNA MUSA</p>
        <p class="brand-sub" style="opacity:0;animation:introMusaFade 1s ease forwards .2s;">Team Fernandes</p>
        <p class="sub" style="margin:24px 0 8px;font-size:16px;color:var(--text);">Que bom ter você aqui</p>
        <p class="page-sub" style="margin-bottom:28px;">A partir de agora, seu treino, sua evolução e seus indicadores vivem num só lugar, pensado pra você.</p>

        <div class="info-box" style="text-align:left;margin-bottom:14px;cursor:pointer;" onclick="finalizarOnboardingEIr('treino')">
          <p class="txt" style="font-weight:600;margin-bottom:4px;"><i class="ti ti-clipboard-list" style="color:var(--gold-soft);margin-right:8px;"></i>Seu treino, sempre atualizado</p>
          <p class="txt" style="margin-bottom:0;color:var(--text-faint);">Construído sob medida pra você, evoluindo junto com você a cada ciclo.</p>
        </div>
        <div class="info-box" style="text-align:left;margin-bottom:14px;cursor:pointer;" onclick="finalizarOnboardingEIr('dna')">
          <p class="txt" style="font-weight:600;margin-bottom:4px;"><i class="ti ti-dna-2" style="color:var(--gold-soft);margin-right:8px;"></i>Seu DNA MUSA</p>
          <p class="txt" style="margin-bottom:0;color:var(--text-faint);">Indicadores que mostram sua evolução de verdade, não só a balança.</p>
        </div>
        <div class="info-box" style="text-align:left;margin-bottom:24px;cursor:pointer;" onclick="finalizarOnboardingEIr('sol')">
          <p class="txt" style="font-weight:600;margin-bottom:4px;"><i class="ti ti-message-circle-2" style="color:var(--gold-soft);margin-right:8px;"></i>Sol, sempre por perto</p>
          <p class="txt" style="margin-bottom:0;color:var(--text-faint);">Tire dúvidas sobre seu treino a qualquer momento.</p>
        </div>

        <button class="btn-gold" onclick="finalizarOnboarding()">Começar</button>
      </div>
    </div>

    <!-- LAUNCHER -->
    <div class="view" data-view="launcher">
      <div class="launcher">
        <p class="brand" id="launcher-brand" style="opacity:0;animation:introMusaFade 1s ease forwards;">DNA MUSA</p>
        <p class="brand-sub" style="animation:introMusaFade 1s ease forwards .2s;opacity:0;">Team Fernandes</p>
        <p class="sub">Escolha uma opção</p>
        <div class="profiles" style="flex-direction:column;align-items:center;gap:10px;">
          <div class="estrela-launcher">
            <div class="satelite satelite-topo" onclick="openLevel2('dados')">
              <div class="avatar gold" id="avatar-composicao"><span style="font-size:14px;font-weight:700;color:var(--text-dim);">--</span></div>
              <p class="profile-name">Composição</p>
            </div>
            <div class="satelite satelite-esquerda" onclick="openLevel2('mentoria')">
              <div class="avatar gold"><span style="color:var(--text-dim);font-size:18px;">M</span></div>
              <p class="profile-name">Mentoria</p>
            </div>
            <div class="satelite satelite-direita" onclick="openLevel2('progresso')">
              <div class="avatar gold"><i class="ti ti-trending-up" style="font-size:20px;color:var(--text-dim);"></i></div>
              <p class="profile-name">Progresso</p>
            </div>
            <div class="satelite satelite-baixo" onclick="openLevel2('ranking')">
              <div class="avatar gold"><i class="ti ti-trophy" style="font-size:20px;color:var(--text-dim);"></i></div>
              <p class="profile-name">Ranking</p>
            </div>
            <div class="satelite-centro" onclick="openLevel2('home')">
              <div class="avatar gold" id="avatar-launcher" style="width:78px;height:78px;box-shadow:0 0 0 4px rgba(232,197,138,0.15);"><span style="font-size:28px;">A</span></div>
              <p class="profile-name" id="nome-aluna-launcher-centro" style="font-size:13px;font-weight:600;">Andriele</p>
            </div>
          </div>
          <div class="profile-card" id="card-personal-launcher" onclick="openLevel2('personal')">
            <div class="avatar dark admin"><i class="ti ti-user-cog"></i></div>
            <p class="profile-name">Personal</p>
            <p class="profile-desc">Painel administrativo</p>
          </div>
          <p style="font-size:11px;color:var(--text-faint);cursor:pointer;margin-top:6px;" onclick="confirmarSairDaConta()"><i class="ti ti-logout" style="margin-right:4px;"></i>Sair da conta</p>
        </div>
      </div>
    </div>

    <!-- HOME (aluna) -->
    <div class="view" data-view="home">
      <div style="display:flex;justify-content:space-between;align-items:flex-start;margin-bottom:20px;">
        <div>
          <p style="font-family:'Playfair Display',serif;font-size:22px;font-weight:700;color:var(--gold);margin:0;letter-spacing:0.5px;">DNA MUSA</p>
          <p style="font-size:9.5px;letter-spacing:2px;color:var(--text-faint);text-transform:uppercase;margin:2px 0 0;">Team Fernandes</p>
        </div>
        <div style="display:flex;gap:8px;align-items:center;position:relative;">
          <div style="width:36px;height:36px;border-radius:50%;background:var(--card-2);border:1px solid var(--border);display:flex;align-items:center;justify-content:center;cursor:pointer;flex-shrink:0;position:relative;" onclick="togglePainelAvisos()">
            <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="var(--gold-soft)" stroke-width="1.3" stroke-linecap="round" stroke-linejoin="round"><path d="M10 5a2 2 0 0 1 4 0a7 7 0 0 1 4 6v3a4 4 0 0 0 2 3h-16a4 4 0 0 0 2 -3v-3a7 7 0 0 1 4 -6"/><path d="M9 17v1a3 3 0 0 0 6 0v-1"/></svg>
            <span id="badge-avisos" style="display:none;position:absolute;top:-2px;right:-2px;background:var(--gold-soft);color:#1A1409;font-size:9px;font-weight:700;min-width:16px;height:16px;border-radius:8px;align-items:center;justify-content:center;padding:0 3px;"></span>
          </div>
          <div id="painel-avisos-aluna" style="display:none;position:absolute;top:44px;right:0;width:280px;max-height:320px;overflow-y:auto;background:var(--card);border:1px solid var(--border-strong);border-radius:14px;padding:10px;z-index:50;box-shadow:0 10px 30px rgba(0,0,0,0.5);"></div>
          <div style="width:36px;height:36px;border-radius:50%;background:var(--card-2);border:1px solid var(--border);display:flex;align-items:center;justify-content:center;cursor:pointer;flex-shrink:0;" onclick="openLevel2('launcher')">
            <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="var(--gold-soft)" stroke-width="1.3" stroke-linecap="round" stroke-linejoin="round"><path d="M20 21v-2a4 4 0 0 0 -4 -4h-8a4 4 0 0 0 -4 4v2"/><circle cx="12" cy="7" r="4"/></svg>
          </div>
        </div>
      </div>
      <h1 class="page-title">Olá, Andriele</h1>
      <p class="page-sub">Sua Central de Inteligência</p>
      <div style="width:36px;height:2px;background:linear-gradient(90deg,var(--gold),transparent);margin:8px 0 26px;"></div>

      <div class="dna-score-card" onclick="openDetail('dna')">
        <div class="dna-score-helix">
          <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="var(--gold-soft)" stroke-width="1.2" stroke-linecap="round" stroke-linejoin="round">
            <path d="M7 2.5c0 5 10 4.5 10 9.5s-10 4.5 -10 9.5"/>
            <path d="M17 2.5c0 5 -10 4.5 -10 9.5s10 4.5 10 9.5"/>
            <path d="M8 6.2h8M7.3 12h9.4M8 17.8h8"/>
          </svg>
        </div>
        <div class="dna-score-left">
          <p class="dna-score-label">DNA SCORE</p>
          <p class="dna-score-numero" id="home-score-numero">--</p>
          <p class="dna-score-comparativo" id="home-score-comparativo"></p>
          <span class="dna-score-ver-detalhes">Ver detalhes do DNA <svg width="11" height="11" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M9 6l6 6l-6 6"/></svg></span>
        </div>
        <div class="dna-score-ring">
          <svg width="76" height="76" viewBox="0 0 96 96">
            <circle cx="48" cy="48" r="40" fill="none" stroke="rgba(232,197,138,0.18)" stroke-width="6.5"/>
            <circle id="home-score-ring-progresso" cx="48" cy="48" r="40" fill="none" stroke="url(#homeRingGrad)" stroke-width="6.5" stroke-linecap="round" stroke-dasharray="251" stroke-dashoffset="251" transform="rotate(-90 48 48)"/>
            <defs><linearGradient id="homeRingGrad" x1="0" y1="0" x2="1" y2="1"><stop offset="0%" stop-color="#F4D9A5"/><stop offset="100%" stop-color="#E8C58A"/></linearGradient></defs>
          </svg>
          <span class="dna-score-ring-valor" id="home-score-ring-valor">--</span>
        </div>
      </div>

      <p class="section-label" style="margin-top:22px;">Seus indicadores</p>
      <div class="indicadores-grid" id="home-indicadores-grid"></div>

      <div class="list-item" style="cursor:pointer;background:linear-gradient(135deg,var(--gold-soft),#B4741F);border:none;margin-top:18px;" onclick="abrirListaDeTreinosDaSemana()">
        <span style="color:#1A1409;font-weight:600;"><i class="ti ti-calendar-week" style="margin-right:8px;"></i>Ver treinos da semana</span>
        <span class="tag" id="resumo-dias-semana" style="background:rgba(0,0,0,0.15);color:#1A1409;"></span>
      </div>

      <div class="list-item" style="cursor:pointer;margin-top:10px;" onclick="openLevel2('progresso')">
        <span><i class="ti ti-trending-up" style="margin-right:8px;color:var(--gold-soft);"></i>Meu progresso</span>
        <i class="ti ti-chevron-right" style="color:var(--text-faint);"></i>
      </div>
    </div>

    <div id="botao-flutuante-sol" style="display:none;position:fixed;bottom:90px;right:16px;width:52px;height:52px;border-radius:50%;background:linear-gradient(135deg,#F4D9A5,#E8C58A);display:flex;align-items:center;justify-content:center;cursor:pointer;box-shadow:0 4px 14px rgba(0,0,0,0.4);z-index:500;" onclick="openLevel2('chatia')">
      <i class="ti ti-message-circle-2" style="color:#1A1409;font-size:22px;"></i>
    </div>

    <!-- DADOS (composição corporal) -->
    <div class="view" data-view="dados">
      <h1 class="page-title">Composição corporal</h1>
      <p class="page-sub">Seus dados de peso e composição, atualizados por você</p>

      <div class="stat-grid">
        <div class="stat-card">
          <p class="stat-label">Peso</p>
          <p class="stat-value" id="stat-peso-atual">--</p>
          <p class="stat-meta" id="stat-peso-data">Ainda não informado</p>
        </div>
        <div class="stat-card">
          <p class="stat-label">% gordura</p>
          <p class="stat-value" id="stat-gordura-atual">--</p>
          <p class="stat-meta gold" id="stat-gordura-data">Ainda não informado</p>
        </div>
      </div>

      <button class="btn-gold" style="margin-top:12px;" onclick="iniciarRegistroComposicaoNova()"><i class="ti ti-clipboard-plus" style="margin-right:6px;"></i>Registrar novas informações</button>
      <div id="area-registro-composicao"></div>

      <p class="section-label" style="margin-top:22px;">Avaliações</p>
      <div class="row" id="row-avaliacao"></div>

      <p class="section-label" style="margin-top:22px;">Minha evolução</p>
      <div id="area-minha-evolucao"></div>
    </div>

    <!-- MEU PROGRESSO -->
    <div class="view" data-view="progresso">
      <h1 class="page-title">Meu progresso</h1>
      <p class="page-sub">Sua evolução desde o início</p>
      <div id="area-progresso-conteudo"></div>
    </div>

    <!-- RANKING -->
    <div class="view" data-view="ranking">
      <h1 class="page-title">Ranking da comunidade</h1>
      <p class="page-sub">Todo mundo junto, evoluindo junto</p>
      <div id="area-ranking-conteudo"></div>
    </div>

    <!-- RODA DA VIDA -->
    <div class="view" data-view="rodadavida">
      <h1 class="page-title">Roda da vida</h1>
      <p class="page-sub">Avalie 10 áreas da sua vida, uma vez por mês</p>
      <div id="area-rodadavida-conteudo"></div>
    </div>

    <!-- MENTORIA -->
    <div class="view" data-view="mentoria">
      <h1 class="page-title">Mentoria</h1>
      <p class="page-sub">Cursos e conteúdos completos</p>

      <p class="section-label">Suporte e técnica</p>
      <p class="page-sub" style="margin-top:-6px;">Sugestões rápidas</p>
      <div class="row" id="row-suporte"></div>

      <p class="section-label" style="margin-top:16px;">Dicas rápidas</p>
      <div class="row" id="row-dicas"></div>

      <p class="section-label" style="margin-top:16px;">Biblioteca completa</p>
      <div class="filter-row" id="filter-row"></div>
      <div class="poster-grid" id="poster-grid"></div>
    </div>

    <!-- PERSONAL -->
    <div class="view" data-view="personal">

      <div id="personal-dashboard">
        <h1 class="page-title">Painel do Personal</h1>
        <p class="page-sub">Visão geral das suas alunas</p>

        <div id="metricas-negocio-area"></div>

        <button class="btn-gold" style="margin-bottom:14px;margin-top:10px;" onclick="iniciarGeracaoEmMassa()"><i class="ti ti-bolt" style="margin-right:6px;"></i>Gerar/progredir treino de todas</button>
        <div id="replicas-treino-area"></div>
        <div id="config-ranking-area"></div>
        <div id="geracao-massa-area"></div>

        <div class="section-colapsavel" style="margin-top:22px;">
          <div style="display:flex;justify-content:space-between;align-items:center;cursor:pointer;" onclick="alternarSecaoColapsavel('dash-comunicacao')">
            <p class="section-label" style="margin:0;">Comunicação com as alunas</p>
            <i class="ti ti-chevron-down" id="chevron-dash-comunicacao" style="color:var(--gold-soft);font-size:16px;transition:transform .2s;"></i>
          </div>
          <div id="conteudo-dash-comunicacao" style="display:none;margin-top:8px;">
            <p class="section-label" style="margin-top:0;">Central de avisos (WhatsApp)</p>
            <button class="chip" style="cursor:pointer;margin-bottom:10px;" onclick="testarConexaoWhatsApp()"><i class="ti ti-plug-connected" style="margin-right:4px;"></i>Testar conexão do WhatsApp</button>
            <div id="resultado-teste-whatsapp"></div>
            <div id="central-avisos-area"></div>

            <p class="section-label" style="margin-top:22px;">Avisos dentro do app</p>
            <p class="page-sub" style="margin-top:-6px;">Não depende de WhatsApp nem de nenhum serviço externo</p>
            <button class="btn-gold" style="background:var(--card-2);color:var(--gold-soft);border:1px solid var(--border);margin-bottom:10px;" onclick="mostrarFormularioAvisoEmMassa()"><i class="ti ti-bell" style="margin-right:6px;"></i>Mandar aviso pra todas (in-app)</button>
            <div id="aviso-massa-inapp-area"></div>

            <button class="btn-gold" style="background:var(--card-2);color:var(--gold-soft);border:1px solid var(--border);margin-bottom:14px;margin-top:10px;" onclick="mostrarFormularioMensagemEmMassa()"><i class="ti ti-speakerphone" style="margin-right:6px;"></i>Mandar mensagem pra todas</button>
            <div id="mensagem-massa-area"></div>

            <button class="btn-gold" style="background:var(--card-2);color:var(--gold-soft);border:1px solid var(--border);margin-bottom:14px;" onclick="mostrarFormularioAvisarTreinoAjustado()"><i class="ti ti-barbell" style="margin-right:6px;"></i>Avisar treino ajustado (escolher alunas)</button>
            <div id="avisar-treino-ajustado-area"></div>
          </div>
        </div>

        <div class="section-colapsavel" style="margin-top:14px;">
          <div style="display:flex;justify-content:space-between;align-items:center;cursor:pointer;" onclick="alternarSecaoColapsavel('dash-ferramentas-treino')">
            <p class="section-label" style="margin:0;">Ferramentas de treino</p>
            <i class="ti ti-chevron-down" id="chevron-dash-ferramentas-treino" style="color:var(--gold-soft);font-size:16px;transition:transform .2s;"></i>
          </div>
          <div id="conteudo-dash-ferramentas-treino" style="display:none;margin-top:8px;">
            <button class="btn-gold" style="background:var(--card-2);color:var(--gold-soft);border:1px solid var(--border);margin-bottom:14px;" onclick="verificarReplicasDeTreino()"><i class="ti ti-copy-check" style="margin-right:6px;"></i>Verificar réplicas de treino</button>
            <button class="btn-gold" style="background:var(--card-2);color:var(--gold-soft);border:1px solid var(--border);margin-bottom:14px;" onclick="auditarVolumePosteriores()"><i class="ti ti-clipboard-check" style="margin-right:6px;"></i>Auditar volume mínimo de posteriores</button>
            <div id="auditoria-posteriores-area"></div>
            <button class="btn-gold" style="background:var(--card-2);color:var(--gold-soft);border:1px solid var(--border);" onclick="mostrarConfigRanking()"><i class="ti ti-trophy" style="margin-right:6px;"></i>Configurar meta do Ranking</button>
          </div>
        </div>

        <div class="section-colapsavel" style="margin-top:14px;">
          <div style="display:flex;justify-content:space-between;align-items:center;cursor:pointer;" onclick="alternarSecaoColapsavel('dash-inteligencia')">
            <p class="section-label" style="margin:0;">Inteligência de mercado</p>
            <i class="ti ti-chevron-down" id="chevron-dash-inteligencia" style="color:var(--gold-soft);font-size:16px;transition:transform .2s;"></i>
          </div>
          <div id="conteudo-dash-inteligencia" style="display:none;margin-top:8px;">
            <p class="page-sub" style="margin-top:-6px;">O que está bombando agora no nicho de estética feminina/fitness</p>
            <button class="btn-gold" style="background:var(--card-2);color:var(--gold-soft);border:1px solid var(--border);margin-bottom:12px;" onclick="gerarRelatorioTendencias()"><i class="ti ti-trending-up" style="margin-right:6px;"></i>🔎 Gerar novo relatório de tendências</button>
            <div id="relatorio-tendencias-area"></div>
          </div>
        </div>

        <p class="section-label" id="label-ferramentas-dashboard" style="margin-top:22px;">Ferramentas</p>
        <div class="poster-grid" id="grid-ferramentas-personal"></div>
      </div>

      <div id="personal-alunas" style="display:none;">
        <div class="local-back" onclick="showPersonalView('dashboard')"><i class="ti ti-arrow-left"></i><span>Painel</span></div>
        <h1 class="page-title" style="margin-top:0;">Alunas</h1>
        <p class="page-sub" id="alunas-count-label" style="margin-top:-6px;"></p>
        <div id="alertas-risco-emocional-area"></div>
        <div id="aviso-modo-antigas" style="display:none;background:var(--card-2);border:1px solid var(--border-strong);border-radius:10px;padding:10px 12px;margin-bottom:12px;align-items:center;justify-content:space-between;">
          <span style="font-size:12px;color:var(--gold-soft);">Mostrando só alunas antigas (anamnese de 2024 ou antes)</span>
          <span class="chip" style="cursor:pointer;" onclick="voltarParaTodasAlunas()">Ver todas</span>
        </div>
        <div class="filter-row" style="margin-bottom:12px;">
          <div class="chip active" id="filtro-tab-ativas" onclick="alternarFiltroAlunas('ativas')">Ativas</div>
          <div class="chip" id="filtro-tab-porvencer" onclick="alternarFiltroAlunas('porvencer')">Por vencer</div>
          <div class="chip" id="filtro-tab-vencidas" onclick="alternarFiltroAlunas('vencidas')">Vencidas</div>
          <div class="chip" style="background:var(--card-2);color:var(--gold-soft);" onclick="abrirTelaFunil()"><i class="ti ti-filter-cog" style="font-size:12px;vertical-align:-1px;margin-right:4px;"></i>Funil</div>
          <div class="chip" id="chip-modo-selecao" style="background:var(--card-2);color:var(--gold-soft);" onclick="alternarModoSelecaoAlunas()"><i class="ti ti-checkbox" style="font-size:12px;vertical-align:-1px;margin-right:4px;"></i>Marcar</div>
        </div>
        <div id="barra-selecao-alunas" style="display:none;background:var(--card);border:1px solid var(--border-strong);border-radius:12px;padding:10px 12px;margin-bottom:12px;align-items:center;gap:8px;flex-wrap:wrap;">
          <span id="contagem-selecao-alunas" style="font-size:12px;color:var(--gold-soft);flex-shrink:0;">0 selecionada(s)</span>
          <select class="form-select" id="destino-mover-alunas" style="flex:1;min-width:140px;font-size:12px;padding:6px;">
            <option value="ativas">Mover pra Ativas</option>
            <option value="porvencer">Mover pra Por vencer</option>
            <option value="vencidas">Mover pra Vencidas</option>
          </select>
          <span class="chip" style="cursor:pointer;background:var(--gold-soft);color:#1A1409;" onclick="confirmarMoverAlunasSelecionadas()">Mover</span>
          <span class="chip" style="cursor:pointer;" onclick="alternarModoSelecaoAlunas()">Cancelar</span>
        </div>
        <div class="form-group"><input class="form-input" id="aluna-search" placeholder="Buscar aluna por nome..." oninput="renderAlunas()"></div>
        <div id="alunas-list"></div>
      </div>

      <div id="personal-funil" style="display:none;">
        <div class="local-back" onclick="showPersonalView('alunas')"><i class="ti ti-arrow-left"></i><span>Alunas</span></div>
        <h1 class="page-title" style="margin-top:0;">Funil de relacionamento</h1>
        <p class="page-sub" style="margin-top:-6px;">Em qual momento do funil cada aluna está, pra guiar os disparos de mensagem</p>
        <div class="filter-row" style="margin-bottom:14px;">
          <div class="chip active" id="funil-tab-inicio" onclick="alternarEstagioFunil('inicio')">Início</div>
          <div class="chip" id="funil-tab-meio" onclick="alternarEstagioFunil('meio')">Meio</div>
          <div class="chip" id="funil-tab-fim" onclick="alternarEstagioFunil('fim')">Fim</div>
        </div>
        <div id="funil-lista-alunas"></div>
        <p class="section-label" style="margin-top:22px;">Notificações de mensagens desse estágio</p>
        <p class="page-sub" style="margin-top:-6px;">Confira se o conteúdo bate com a metodologia antes de considerar disparado de verdade</p>
        <div id="funil-notificacoes"></div>
      </div>

      <div id="personal-aluna" style="display:none;">
        <div class="local-back" onclick="showPersonalView('alunas')"><i class="ti ti-arrow-left"></i><span>Alunas</span></div>
        <div id="aluna-detail-content"></div>
      </div>

      <div id="personal-resumo-aluna" style="display:none;">
        <div class="local-back" onclick="showPersonalView('aluna')"><i class="ti ti-arrow-left"></i><span>Voltar pra ficha</span></div>
        <div id="resumo-aluna-content"></div>
      </div>

      <div id="personal-exercicios" style="display:none;">
        <div class="local-back" onclick="showPersonalView('dashboard')"><i class="ti ti-arrow-left"></i><span>Painel</span></div>
        <h1 class="page-title" style="margin-top:0;">Banco de exercícios</h1>
        <p class="page-sub" id="ex-count-label">Organizado por grupo muscular</p>

        <div class="filter-row" style="margin-top:10px;">
          <div class="chip active" id="subaba-videos-btn" onclick="alternarSubabaExercicios('videos')">Vídeos</div>
          <div class="chip" id="subaba-metodos-btn" onclick="alternarSubabaExercicios('metodos')">Métodos</div>
        </div>

        <div id="subaba-metodos-view" style="display:none;margin-top:14px;"></div>

        <div id="subaba-videos-container">

        <div id="ex-video-view" style="display:none;margin-top:16px;">
          <div class="local-back" onclick="fecharVideoExercicio()"><i class="ti ti-arrow-left"></i><span>Voltar à lista</span></div>
          <div id="ex-video-content"></div>
        </div>

        <div id="ex-grupos-view" style="margin-top:16px;">
          <div id="ex-grupos-lista"></div>
        </div>

        <div id="ex-lista-view" style="display:none;margin-top:16px;">
          <div class="local-back" onclick="voltarParaGruposExercicios()"><i class="ti ti-arrow-left"></i><span>Grupos musculares</span></div>
          <p class="section-label" id="ex-grupo-titulo" style="margin-top:10px;"></p>
          <div id="ex-add-form" style="display:none;">
            <p class="section-label">Adicionar exercício</p>
            <div class="form-group"><label class="form-label">Nome do exercício</label><input class="form-input" id="ex-nome" placeholder="Ex: Cadeira flexora"></div>
            <div class="form-group"><label class="form-label">Grupo muscular</label>
              <select class="form-select" id="ex-grupo"></select>
            </div>
            <div class="form-group"><label class="form-label">Nível de complexidade</label>
              <select class="form-select" id="ex-nivel"><option>Básico</option><option>Intermediário</option><option>Avançado</option></select>
            </div>
            <div class="form-group"><label class="form-label">Método padrão</label>
              <select class="form-select" id="ex-metodo"><option>Nenhum</option><option>Restpause</option><option>Dropset</option><option>Cluster set</option><option>Bi-set</option><option>Tri-set</option><option>Pirâmide crescente</option></select>
            </div>
            <div class="form-group"><label class="form-label">Link do vídeo (opcional por enquanto)</label><input class="form-input" id="ex-video" placeholder="Cole o link quando tiver disponível"></div>
            <button class="btn-gold" onclick="addExercicio()">Adicionar ao banco</button>
          </div>
          <input class="form-input" id="ex-busca-input" style="margin-bottom:10px;" placeholder="Buscar exercício nesse grupo..." oninput="buscarExerciciosNoGrupo(this.value)">
          <div id="ex-lista"></div>
          <div id="ex-paginacao" style="display:flex;gap:6px;justify-content:center;margin-top:14px;"></div>
        </div>

        </div>
      </div>

      <div id="personal-conteudo" style="display:none;">
        <div class="local-back" onclick="showPersonalView('dashboard')"><i class="ti ti-arrow-left"></i><span>Painel</span></div>
        <h1 class="page-title" style="margin-top:0;">Biblioteca de conteúdo</h1>
        <p class="page-sub">Isso alimenta diretamente a Mentoria das alunas</p>

        <button class="btn-gold" style="background:var(--card-2);color:var(--gold-soft);border:1px solid var(--border);margin-bottom:14px;" onclick="mostrarFormularioCurso()"><i class="ti ti-school" style="margin-right:6px;"></i>Criar curso</button>
        <div id="cursos-list-personal"></div>
        <div id="form-curso-area"></div>

        <p class="section-label" style="margin-top:18px;">Todo o conteúdo (vídeos soltos)</p>
        <div id="conteudo-list-personal"></div>

        <p class="section-label" style="margin-top:18px;">Adicionar conteúdo</p>
        <div class="form-group"><label class="form-label">Título</label><input class="form-input" id="ct-titulo" placeholder="Ex: Execução: stiff"></div>
        <div class="form-group"><label class="form-label">Categoria</label>
          <select class="form-select" id="ct-categoria"><option>Cursos completos</option><option>Introdução</option><option>Métodos de treino</option><option>Exercícios de coxa</option><option>Exercícios de glúteo</option></select>
        </div>
        <div class="form-group"><label class="form-label">Descrição</label><input class="form-input" id="ct-desc" placeholder="Breve descrição do conteúdo"></div>
        <div class="form-group"><label class="form-label">Link do vídeo</label><input class="form-input" id="ct-video" placeholder="Cole o link quando tiver disponível"></div>
        <div class="form-group" style="display:flex;align-items:center;gap:8px;">
          <input type="checkbox" id="ct-locked" style="width:16px;height:16px;">
          <label class="form-label" style="margin:0;" for="ct-locked">Conteúdo bloqueado (pago)</label>
        </div>
        <button class="btn-gold" onclick="addConteudo()">Adicionar à biblioteca</button>
      </div>

      <div id="personal-mobilidade" style="display:none;">
        <div class="local-back" onclick="showPersonalView('dashboard')"><i class="ti ti-arrow-left"></i><span>Painel</span></div>
        <h1 class="page-title" style="margin-top:0;">Biblioteca de mobilidade</h1>
        <p class="page-sub" id="mob-count-label">Mobilidades, alongamentos e aquecimento</p>

        <div id="mob-articulacoes-view">
          <div id="mob-articulacao-lista"></div>
          <p class="section-label" style="margin-top:18px;">Aquecimento</p>
          <div id="aquecimento-list-personal"></div>
        </div>

        <div id="mob-lista-view" style="display:none;">
          <div class="local-back" onclick="voltarParaArticulacoes()"><i class="ti ti-arrow-left"></i><span>Articulações</span></div>
          <p class="section-label" id="mob-articulacao-titulo" style="margin-top:10px;"></p>
          <div class="form-group"><input class="form-input" id="mob-search" placeholder="Buscar por nome ou grupo ativo..." oninput="renderMobilidadeBanco()"></div>
          <div id="mobilidade-list-personal"></div>
        </div>
      </div>

      <div id="personal-treinos" style="display:none;">
        <div class="local-back" onclick="showPersonalView('dashboard')"><i class="ti ti-arrow-left"></i><span>Painel</span></div>
        <h1 class="page-title" style="margin-top:0;">Biblioteca de treinos</h1>
        <p class="page-sub">Modelos prontos por nível, ênfase e frequência</p>

        <div class="form-group"><label class="form-label">Nível</label>
          <select class="form-select" id="tp-filtro-nivel" onchange="renderTemplates()">
            <option>Todos</option><option>Iniciante</option><option>Intermediário</option><option>Avançado</option>
          </select>
        </div>
        <div class="form-group"><label class="form-label">Ênfase</label>
          <select class="form-select" id="tp-filtro-enfase" onchange="renderTemplates()">
            <option>Todas</option><option>Glúteo</option><option>Quadríceps</option><option>Posterior</option><option>Emagrecimento</option>
          </select>
        </div>
        <div class="form-group"><label class="form-label">Frequência</label>
          <select class="form-select" id="tp-filtro-freq" onchange="renderTemplates()">
            <option>Todas</option><option>3x</option><option>5x</option><option>6x</option><option>7x</option>
          </select>
        </div>

        <div id="templates-list"></div>

        <p class="section-label" style="margin-top:18px;">Criar novo template</p>
        <div class="form-group"><label class="form-label">Nível</label>
          <select class="form-select" id="tp-nivel" onchange="updateVolumeHint()">
            <option>Iniciante</option><option>Intermediário</option><option>Avançado</option>
          </select>
        </div>
        <div class="form-group"><label class="form-label">Ênfase</label><input class="form-input" id="tp-enfase" placeholder="Ex: Glúteo"></div>
        <div class="form-group"><label class="form-label">Frequência semanal</label>
          <select class="form-select" id="tp-freq"><option>3x</option><option>5x</option><option>6x</option><option>7x</option></select>
        </div>
        <div class="form-group"><label class="form-label">Volume recomendado (regra por nível)</label>
          <div class="list-item" id="tp-volume-hint" style="cursor:default;"><span>Até 20 séries</span></div>
        </div>
        <div class="form-group"><label class="form-label">Adicionar exercício do banco</label>
          <select class="form-select" id="tp-exercicio-select"></select>
        </div>
        <button class="btn-gold" style="background:var(--card-2);color:var(--gold-soft);border:1px solid var(--border);" onclick="addExercicioAoTemplate()">+ Adicionar à lista</button>
        <div class="chip-list" id="tp-exercicios-selecionados" style="margin:12px 0;"></div>
        <button class="btn-gold" onclick="salvarTemplate()">Salvar template</button>
      </div>

      <div id="personal-desafios" style="display:none;">
        <div class="local-back" onclick="showPersonalView('dashboard')"><i class="ti ti-arrow-left"></i><span>Painel</span></div>
        <h1 class="page-title" style="margin-top:0;">Grupo de desafio</h1>
        <p class="page-sub">Crie um treino e gere um link de inscrição, quem entra participa só do desafio, sem acesso ao restante do app</p>

        <div id="desafios-list"></div>

        <p class="section-label" style="margin-top:18px;">Criar novo desafio</p>
        <div class="form-group"><label class="form-label">Nome do desafio</label><input class="form-input" id="df-nome" placeholder="Ex: Desafio 7 Dias Glúteo de Aço"></div>
        <div class="form-group"><label class="form-label">Treino vinculado</label>
          <select class="form-select" id="df-treino"></select>
        </div>
        <div class="form-group"><label class="form-label">Duração (dias)</label><input class="form-input" id="df-duracao" placeholder="Ex: 7" type="number"></div>
        <button class="btn-gold" onclick="criarDesafio()">Criar desafio e gerar link</button>
      </div>

      <div id="personal-patologias" style="display:none;">
        <div class="local-back" onclick="showPersonalView('dashboard')"><i class="ti ti-arrow-left"></i><span>Painel</span></div>
        <h1 class="page-title" style="margin-top:0;">Banco de patologias</h1>
        <p class="page-sub">Consulta de referência, nunca ativa sozinho, sempre depende da sua confirmação na ficha da aluna</p>
        <div class="filter-row" id="patologia-filter-row"></div>
        <div id="patologia-list"></div>
      </div>

      <div id="personal-desvios" style="display:none;">
        <div class="local-back" onclick="showPersonalView('dashboard')"><i class="ti ti-arrow-left"></i><span>Painel</span></div>
        <h1 class="page-title" style="margin-top:0;">Banco de desvios posturais</h1>
        <p class="page-sub">Consulta de referência, a ativação de verdade acontece marcando na ficha de cada aluna</p>
        <div id="desvios-list"></div>
      </div>

      <div id="personal-corrida" style="display:none;">
        <div class="local-back" onclick="showPersonalView('dashboard')"><i class="ti ti-arrow-left"></i><span>Painel</span></div>
        <h1 class="page-title" style="margin-top:0;">Banco de testes de corrida</h1>
        <p class="page-sub">Testes de avaliação e tipos de treino pra alunos corredores</p>
        <div id="corrida-list"></div>
      </div>

    </div>

    <!-- LISTA DE TREINOS DA SEMANA -->

    <div class="view" data-view="semana-treinos">
      <h1 class="page-title">Treinos da semana</h1>
      <p class="page-sub">Toque num dia pra abrir o treino completo</p>
      <div class="info-box" style="margin-bottom:14px;">
        <p class="lbl" style="margin-bottom:6px;">Como funciona esse treino</p>
        <p class="txt" style="margin-bottom:0;">Faça as séries de cada exercício na ordem. Registre a carga e as repetições da <b>1ª série</b>, é isso que ajusta sua carga sugerida pra próxima vez. O descanso entre séries já está calculado pra cada exercício, é só seguir o cronômetro.</p>
      </div>
      <div id="lista-dias-semana"></div>
    </div>

    <!-- DETAIL -->

    <div class="view" data-view="detail">
      <div id="detail-content"></div>
    </div>

    <!-- CHAT IA -->
    <div class="view" data-view="chatia">
      <h1 class="page-title">Pergunte à Sol</h1>
      <p class="page-sub">Sua treinadora digital, sempre disponível</p>
      <div id="chatia-mensagens" style="display:flex;flex-direction:column;gap:10px;margin:16px 0;min-height:200px;"></div>
      <div id="chatia-status" style="font-size:12px;color:var(--text-faint);margin-bottom:8px;"></div>
      <div style="display:flex;gap:8px;">
        <input class="form-input" id="chatia-input" placeholder="Digite sua pergunta..." style="flex:1;" onkeypress="if(event.key==='Enter') enviarMensagemChatIA()">
        <button class="btn-gold" style="width:auto;padding:12px 18px;margin:0;" onclick="enviarMensagemChatIA()">Enviar</button>
      </div>
    </div>

    </div>

    <div id="bottom-nav-fixa" class="bottom-nav-fixa" style="display:none;">
      <div class="bottom-nav-item" data-nav="home" onclick="irParaAbaFixa('home')">
        <svg class="nav-icon-svg" width="22" height="22" viewBox="0 0 24 24"><path d="M2 9h2v6H2zm3-2h2v10H5zm3-1h8v14H8zm9 1h2v10h-2zm3 2h2v6h-2z"/></svg><span>Treino</span>
      </div>
      <div class="bottom-nav-item" data-nav="dados" onclick="irParaAbaFixa('dados')">
        <svg class="nav-icon-svg" width="22" height="22" viewBox="0 0 24 24"><rect x="3" y="12" width="4" height="8" rx="1"/><rect x="10" y="7" width="4" height="13" rx="1"/><rect x="17" y="3" width="4" height="17" rx="1"/></svg><span>Composição</span>
      </div>
      <div class="bottom-nav-item" data-nav="mentoria" onclick="irParaAbaFixa('mentoria')">
        <svg class="nav-icon-svg" width="22" height="22" viewBox="0 0 24 24"><rect x="2" y="4" width="20" height="16" rx="2"/><path d="M10 8.5v7l6-3.5z" fill="var(--bg)"/></svg><span>Mentoria</span>
      </div>
      <div class="bottom-nav-item" data-nav="progresso" onclick="irParaAbaFixa('progresso')">
        <svg class="nav-icon-svg" width="22" height="22" viewBox="0 0 24 24"><path d="M3 17l5-6 4 3 6-8 3 3v10a1 1 0 0 1-1 1H4a1 1 0 0 1-1-1z"/></svg><span>Progresso</span>
      </div>
      <div class="bottom-nav-item" data-nav="ranking" onclick="irParaAbaFixa('ranking')">
        <svg class="nav-icon-svg" width="22" height="22" viewBox="0 0 24 24"><path d="M6 3h12v4a6 6 0 0 1-5 5.92V15h3v2H8v-2h3v-2.08A6 6 0 0 1 6 7zm-4 1h3v3a4 4 0 0 1-3-3zm17 0h3a4 4 0 0 1-3 3z"/></svg><span>Ranking</span>
      </div>
      <div class="bottom-nav-item" data-nav="rodadavida" onclick="irParaAbaFixa('rodadavida')">
        <svg class="nav-icon-svg" width="22" height="22" viewBox="0 0 24 24"><path d="M12 2a10 10 0 1 0 10 10h-2a8 8 0 1 1-8-8z"/><path d="M12 6a6 6 0 1 0 6 6h-2a4 4 0 1 1-4-4z"/><circle cx="12" cy="12" r="2"/></svg><span>Roda da vida</span>
      </div>
    </div>
  </div>
</div>

<div id="tela-erro-generica" style="display:none;position:fixed;inset:0;background:var(--bg);z-index:99999;flex-direction:column;align-items:center;justify-content:center;padding:32px;text-align:center;">
  <div style="width:56px;height:56px;border-radius:50%;background:var(--card-2);border:1px solid var(--border-strong);display:flex;align-items:center;justify-content:center;margin-bottom:18px;">
    <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="var(--gold-soft)" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><path d="M12 9v4"/><path d="M10.363 3.591l-8.106 13.535a1.914 1.914 0 0 0 1.636 2.874h16.214a1.914 1.914 0 0 0 1.636 -2.874l-8.106 -13.535a1.914 1.914 0 0 0 -3.274 0z"/><path d="M12 16h.01"/></svg>
  </div>
  <p style="font-family:'Playfair Display',serif;font-size:19px;font-weight:700;color:var(--text);margin:0 0 8px;">Algo não carregou direito</p>
  <p style="font-size:13px;color:var(--text-dim);margin:0 0 22px;max-width:280px;">Pode ter sido uma queda de internet momentânea. Tenta recarregar — seus dados estão salvos.</p>
  <button class="btn-gold" style="width:auto;padding:12px 28px;" onclick="window.location.reload()">Recarregar</button>
</div>

<script>
window.addEventListener('error', function(evento){
  // Só reage a erro de JavaScript de verdade (tem stack de erro real).
  // Erro de carregar fonte/imagem/CDN não deve travar o app inteiro — isso já é tratado visualmente em cada lugar específico.
  if(!evento.error) return;
  console.error('[Erro capturado]', evento.error);
  const tela = document.getElementById('tela-erro-generica');
  if(tela && tela.style.display !== 'flex') tela.style.display = 'flex';
});
window.addEventListener('unhandledrejection', function(evento){
  console.error('[Promessa rejeitada sem tratamento]', evento.reason);
  // Erros de rede do Supabase já têm tratamento próprio (try/catch), então aqui só logamos,
  // não mostramos a tela cheia pra não interromper por uma falha que o próprio código já absorveu.
});
</script>

<script>
let level2 = null;
let sessaoTipo = null; // 'aluna' | 'personal' — declarado cedo de propósito, é usado logo no primeiro render da tela

const faseAtual = 'Fase 1 · Emagrecimento (override por IMC)';
let dias = [
  {n:'Segunda', foco:'Treino A · Quadríceps e Glúteo', ex:['Leg Press 45 · 4x12','Agachamento Hack · 3x12','Extensão de quadril na máquina · 3x15','Abdução de quadril na polia · 3x15']},
  {n:'Terça', foco:'Treino B · Costas e Abdômen', ex:['Bi-set|||Remada articular aberta · 3x12|||Crucifixo Inverso · 3x15','Remada articular fechada · 3x12','Prancha ventral · 3x30s','Abdominal canivete · 3x15']},
  {n:'Quarta', foco:'Treino C · Glúteo e Posterior', ex:['Extensão de quadril no banco romano · 4x15','Cadeira flexora · 3x12','Leg Press 45 (pés altos) · 3x15','Abdução de quadril na polia · 3x15'], hoje:true},
  {n:'Quinta', foco:'Treino D · Costas e Abdômen', ex:['Remada articular aberta · 3x12','Remada articular fechada · 3x12','Prancha ventral · 3x30s','Abdominal canivete · 3x15']},
  {n:'Sexta', foco:'Treino E · Pernas completo', ex:['Agachamento Hack · 3x12','Leg Press 45 · 3x15','Cadeira flexora · 3x12','Extensão de quadril na máquina · 3x15']},
  {n:'Sábado', foco:'Descanso', descanso:true},
  {n:'Domingo', foco:'Descanso', descanso:true}
];

const mobilidadeItens = [
  {n:'Mobilidade de quadril', dur:'8 min', desvio:null, foco:'Mobilidade geral de quadril, recomendada como rotina inicial até a primeira avaliação postural com fotos.', video:'https://youtu.be/NrF08RNhKyY'},
  {n:'Alongamento pós-treino', dur:'6 min', desvio:null, foco:'Alongamento geral pós-treino, ótimo ponto de partida, já que você disse ter interesse em começar a alongar.', video:null},
  {n:'Respiração e relaxamento', dur:'5 min', desvio:null, foco:'Manobras respiratórias e vácuo abdominal para recuperação e controle de core. Comece pela Parte 1, Aprenda a Respirar. Sequência completa (7 vídeos) na Mentoria.', video:'https://youtu.be/8ixgAPGj2nE'}
];

function poster(container, title, sub, onClick, opts){
  opts = opts || {};
  const el = document.createElement('div');
  el.className = 'poster' + (opts.pendente ? ' pendente' : '');
  el.innerHTML =
    (opts.done ? '<i class="ti ti-check check"></i>' : '') +
    (opts.pendente ? '<span class="pend-tag">pendente</span>' : '') +
    '<p class="sub">' + sub + '</p>' +
    '<p class="title">' + title + '</p>';
  el.onclick = onClick;
  container.appendChild(el);
}

function abrirListaDeTreinosDaSemana(){
  const lista = document.getElementById('lista-dias-semana');
  const abreviacoes = { 'Segunda':'SEG', 'Terça':'TER', 'Quarta':'QUA', 'Quinta':'QUI', 'Sexta':'SEX', 'Sábado':'SÁB', 'Domingo':'DOM' };
  const prog = getProgressoAluna(NOME_ALUNA_LOGADA);
  const diasRegistradosEssaSemana = prog.diasConcluidos[prog.semana] || [];
  lista.innerHTML = dias.map(function(d, i){
    const abrev = abreviacoes[d.n] || d.n.slice(0,3).toUpperCase();
    const foiRegistrado = diasRegistradosEssaSemana.indexOf(d.n) !== -1;
    return '<div style="display:flex;align-items:center;gap:14px;padding:16px;margin-bottom:10px;border-radius:16px;background:' + (foiRegistrado ? 'var(--success-soft)' : (d.hoje ? 'linear-gradient(135deg,rgba(217,139,46,0.18),rgba(92,56,20,0.12))' : 'var(--card)')) + ';border:1px solid ' + (foiRegistrado ? 'var(--success)' : (d.hoje ? 'var(--gold-soft)' : 'var(--border)')) + ';cursor:pointer;" onclick="abrirDiaDaSemana(' + i + ')">' +
      '<div style="width:48px;height:48px;border-radius:14px;background:' + (foiRegistrado ? 'var(--success)' : 'linear-gradient(135deg,#F4D9A5,#E8C58A)') + ';display:flex;align-items:center;justify-content:center;flex-shrink:0;">' +
        '<span style="font-size:11px;font-weight:700;letter-spacing:0.5px;color:#1A1409;">' + abrev + '</span>' +
      '</div>' +
      '<div style="flex:1;min-width:0;">' +
        '<p style="font-size:13px;font-weight:600;margin:0 0 2px;">' + d.n + (d.hoje ? ' <span class="tag" style="margin-left:4px;">hoje</span>' : '') + (foiRegistrado ? ' <span class="tag" style="margin-left:4px;background:var(--success-soft);color:var(--success);">registrado</span>' : '') + '</p>' +
        '<p style="font-size:12px;color:var(--text-faint);margin:0;font-weight:400;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;">' + d.foco + '</p>' +
      '</div>' +
      (foiRegistrado ? '<i class="ti ti-circle-check" style="color:var(--success);font-size:20px;flex-shrink:0;"></i>' : '<i class="ti ti-chevron-right" style="color:var(--text-faint);font-size:18px;flex-shrink:0;"></i>') +
    '</div>';
  }).join('');
  setActive('semana-treinos');
}

function abrirDiaDaSemana(i){
  veioDaListaDeTreinosDaSemana = true;
  setActive('detail');
  document.getElementById('backlabel').textContent = 'Treinos da semana';
  openDetail('dia', i);
}

const resumoDias = document.getElementById('resumo-dias-semana');
if(resumoDias) resumoDias.textContent = dias.length + ' dias';

function calcularComparativoSemanal(nome){
  const prog = getProgressoAluna(nome);
  if(!prog.nutricao) prog.nutricao = {}; // protege alunas com progresso salvo antigo, de antes desse campo existir
  const semanaAtual = prog.semana;
  const semanaAnterior = semanaAtual - 1;
  const resultado = {};

  const totalDia = totalDiasDeTreino();
  if(prog.diasConcluidos[semanaAtual] && prog.diasConcluidos[semanaAnterior] && totalDia > 0){
    const atual = Math.round((prog.diasConcluidos[semanaAtual].length / totalDia) * 100);
    const anterior = Math.round((prog.diasConcluidos[semanaAnterior].length / totalDia) * 100);
    resultado.constancia = atual - anterior;
  }

  let progAtual = 0, progAnterior = 0, temHistoricoAnterior = false;
  Object.keys(prog.historico).forEach(function(ex){
    prog.historico[ex].forEach(function(r){
      if(r.semana === semanaAtual && r.sugestao.texto === 'Aumentar carga') progAtual++;
      if(r.semana === semanaAnterior){
        temHistoricoAnterior = true;
        if(r.sugestao.texto === 'Aumentar carga') progAnterior++;
      }
    });
  });
  if(temHistoricoAnterior) resultado.progressao = progAtual - progAnterior;

  if(prog.nutricao[semanaAtual] && prog.nutricao[semanaAnterior]){
    resultado.nutricao = prog.nutricao[semanaAtual].resultado.respostaNutricional - prog.nutricao[semanaAnterior].resultado.respostaNutricional;
    const hipertrofiaAtual = Math.max(20, Math.min(95, 60 + prog.nutricao[semanaAtual].resultado.ajusteHipertrofia));
    const hipertrofiaAnterior = Math.max(20, Math.min(95, 60 + prog.nutricao[semanaAnterior].resultado.ajusteHipertrofia));
    resultado.hipertrofia = hipertrofiaAtual - hipertrofiaAnterior;
  }

  if(prog.nutricao[semanaAtual] && prog.nutricao[semanaAnterior]
     && prog.nutricao[semanaAtual].resultado.ajusteRecuperacaoSono != null && prog.nutricao[semanaAnterior].resultado.ajusteRecuperacaoSono != null){
    resultado.recuperacao = prog.nutricao[semanaAtual].resultado.ajusteRecuperacaoSono - prog.nutricao[semanaAnterior].resultado.ajusteRecuperacaoSono;
  }

  return resultado;
}

const iconesIndicadorSvg = {
  constancia: '<svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="var(--gold-soft)" stroke-width="1.3" stroke-linecap="round" stroke-linejoin="round"><rect x="4" y="5" width="16" height="16" rx="2"/><path d="M16 3v4M8 3v4M4 11h16"/><path d="M9 16l2 2l4 -4"/></svg>',
  hipertrofia: '<svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="var(--gold-soft)" stroke-width="1.3" stroke-linecap="round" stroke-linejoin="round"><path d="M2 12h1"/><path d="M6 8h-2a1 1 0 0 0 -1 1v6a1 1 0 0 0 1 1h2"/><path d="M6 7v10a1 1 0 0 0 1 1h1a1 1 0 0 0 1 -1v-10a1 1 0 0 0 -1 -1h-1a1 1 0 0 0 -1 1"/><path d="M9 12h6"/><path d="M15 7v10a1 1 0 0 0 1 1h1a1 1 0 0 0 1 -1v-10a1 1 0 0 0 -1 -1h-1a1 1 0 0 0 -1 1"/><path d="M18 8h2a1 1 0 0 1 1 1v6a1 1 0 0 1 -1 1h-2"/><path d="M22 12h-1"/></svg>',
  nutricao: '<svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="var(--gold-soft)" stroke-width="1.3" stroke-linecap="round" stroke-linejoin="round"><path d="M9 7c-3 0 -4 3 -4 5.5c0 3 2 6.5 4 6.5c1.088 -.046 1.679 -.5 3 -.5c1.312 0 1.5 .5 3 .5s4 -3 4 -5c-.028 -.01 -2.472 -.403 -2.5 -3c-.019 -2.17 2.416 -2.954 2.5 -3c-.532 -1.396 -1.545 -1.86 -2 -2c-1.335 -.397 -2.487 .53 -3.5 .5c-.87 -.087 -2.512 -1 -4 -1z"/><path d="M12 4a2 2 0 0 0 2 -2a2 2 0 0 0 -2 2"/></svg>',
  recuperacao: '<svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="var(--gold-soft)" stroke-width="1.3" stroke-linecap="round" stroke-linejoin="round"><path d="M12 3c.132 0 .263 0 .393 0a7.5 7.5 0 0 0 7.92 12.446a9 9 0 1 1 -8.313 -12.454z"/></svg>',
  progressao: '<svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="var(--gold-soft)" stroke-width="1.3" stroke-linecap="round" stroke-linejoin="round"><path d="M3 17l6 -6l4 4l8 -8"/><path d="M14 7l7 0l0 7"/></svg>'
};

function calcularComparativoScore(nome){
  const c = calcularComparativoSemanal(nome);
  const pesos = { constancia: 0.30, progressao: 0.20, recuperacao: 0.15, hipertrofia: 0.15, nutricao: 0.10 };
  let somaPesos = 0, somaPonderada = 0;
  Object.keys(pesos).forEach(function(chave){
    if(c[chave] != null && !isNaN(c[chave])){
      somaPonderada += c[chave] * pesos[chave];
      somaPesos += pesos[chave];
    }
  });
  if(somaPesos < 0.3) return null; // dado de menos pra um comparativo confiável do score geral
  return Math.round(somaPonderada / somaPesos * (somaPesos)); // pondera só pelo que existe, mantendo proporção
}

function qualidadeDoScore(score){
  if(score >= 85) return 'Excelente';
  if(score >= 70) return 'Bom';
  if(score >= 50) return 'Regular';
  return 'Em construção';
}

function renderHome(){
  const stats = calcularEstatisticasAluna(NOME_ALUNA_LOGADA);
  const nutriStats = calcularNutricaoStats(NOME_ALUNA_LOGADA);
  const elNumero = document.getElementById('home-score-numero');
  const elQualidade = document.getElementById('home-score-qualidade');
  const elComparativo = document.getElementById('home-score-comparativo');
  const elRingValor = document.getElementById('home-score-ring-valor');
  const elRingProgresso = document.getElementById('home-score-ring-progresso');
  const grid = document.getElementById('home-indicadores-grid');

  if(!stats.temDados){
    if(elNumero) elNumero.textContent = '--';
    if(elQualidade) elQualidade.textContent = 'Calibrando';
    if(elComparativo) elComparativo.textContent = 'Seus primeiros treinos vão formar esse número';
    if(elRingValor) elRingValor.textContent = '--';
    if(elRingProgresso) elRingProgresso.style.strokeDashoffset = '251';
    if(grid) grid.innerHTML = '<div class="indicador-card" style="grid-column:span 6;"><p class="indicador-caption" style="margin:0;">Complete seu primeiro treino pra começar a ver seus indicadores aqui.</p></div>';
  } else {
    const pctConstancia = Math.round((stats.totalConcluidos / stats.totalPlanejado) * 100);
    const alunaObj = alunasPersonal.find(function(a){ return a.nome === NOME_ALUNA_LOGADA; });
    const ajusteIdade = ajusteRecuperacaoPorIdade(alunaObj ? alunaObj.idade : null);
    const ajusteSono = nutriStats.temDados ? nutriStats.ajusteRecuperacaoSono : 0;
    const capRecuperacao = Math.max(20, Math.min(95, (stats.progressoes >= 2 ? 68 : 60) + ajusteIdade.ajuste + ajusteSono));
    const capProgressao = Math.min(90, 55 + stats.progressoes * 10);
    const score = calcularProbabilidadeSucesso(NOME_ALUNA_LOGADA);

    if(elNumero) elNumero.textContent = score;
    const deltaScore = calcularComparativoScore(NOME_ALUNA_LOGADA);
    if(elComparativo){
      if(deltaScore == null){
        elComparativo.textContent = 'Aguardando dados suficientes';
        elComparativo.style.color = 'var(--text-faint)';
      } else if(deltaScore === 0){
        elComparativo.textContent = 'Estável vs. semana anterior';
        elComparativo.style.color = 'var(--text-faint)';
      } else {
        elComparativo.textContent = (deltaScore > 0 ? '▲ +' : '▼ ') + deltaScore + '% vs. semana anterior';
        elComparativo.style.color = deltaScore > 0 ? 'var(--success)' : 'var(--text-faint)';
      }
    }
    if(elRingValor) elRingValor.textContent = score;
    if(elRingProgresso) elRingProgresso.style.strokeDashoffset = String(251 - (251 * Math.min(score,100) / 100));

    const comparativo = calcularComparativoSemanal(NOME_ALUNA_LOGADA);
    function formatarComparativo(delta){
      if(delta == null || isNaN(delta)) return 'Dados insuficientes';
      if(delta === 0) return 'Estável em relação à semana anterior.';
      const seta = delta > 0 ? '▲' : '▼';
      return seta + ' ' + (delta > 0 ? '+' : '') + delta + '% vs. semana anterior';
    }

    const indicadores = [
      { label: 'Constância', valor: pctConstancia, icone: 'constancia', caption: formatarComparativo(comparativo.constancia) },
      { label: 'Hipertrofia', valor: nutriStats.temDados ? nutriStats.potencialHipertrofia : null, icone: 'hipertrofia', caption: nutriStats.temDados ? formatarComparativo(comparativo.hipertrofia) : 'Dados insuficientes' },
      { label: 'Nutrição', valor: nutriStats.temDados ? nutriStats.respostaNutricional : null, icone: 'nutricao', caption: nutriStats.temDados ? formatarComparativo(comparativo.nutricao) : 'Dados insuficientes' },
      { label: 'Recuperação', valor: capRecuperacao, icone: 'recuperacao', caption: formatarComparativo(comparativo.recuperacao) },
      { label: 'Progressão', valor: capProgressao, icone: 'progressao', caption: formatarComparativo(comparativo.progressao) }
    ];

    if(grid){
      grid.innerHTML = indicadores.map(function(ind, i){
        const ehLargo = i >= 3; // os 2 últimos (Recuperação, Progressão) ficam mais largos, igual ao conceito
        const valorTexto = ind.valor == null ? '--' : ind.valor + '%';
        const larguraBarra = ind.valor == null ? 0 : Math.min(100, ind.valor);
        const corCaption = ind.caption.indexOf('▲') === 0 ? 'var(--success)' : 'var(--text-faint)';
        return '<div class="indicador-card' + (ehLargo ? ' largo' : '') + '" style="animation-delay:' + (0.08 * i) + 's;">' +
          '<div class="indicador-icone">' + iconesIndicadorSvg[ind.icone] + '</div>' +
          '<p class="indicador-label">' + ind.label + '</p>' +
          '<p class="indicador-valor">' + valorTexto + '</p>' +
          '<div class="indicador-barra-bg"><div class="indicador-barra-fill" style="width:' + larguraBarra + '%;"></div></div>' +
          '<p class="indicador-caption" style="color:' + corCaption + ';">' + ind.caption + '</p>' +
        '</div>';
      }).join('');
    }
  }

  const botaoSol = document.getElementById('botao-flutuante-sol');
  if(botaoSol) botaoSol.style.display = (sessaoTipo === 'aluna') ? 'flex' : 'none';
}

function renderAvaliacoes(){
  const rowAval = document.getElementById('row-avaliacao');
  if(!rowAval) return;
  rowAval.innerHTML = '';
  const alunaObj = alunasPersonal.find(function(a){ return a.nome === NOME_ALUNA_LOGADA; });
  let subAval = 'Ainda não realizada';
  if(alunaObj && alunaObj.dataAnamnese){
    const dataBase = new Date(alunaObj.dataAnamnese);
    const hoje = new Date();
    const diasPassados = Math.floor((hoje - dataBase) / (1000*60*60*24));
    const dataProxima = new Date(dataBase.getTime() + 90*24*60*60*1000);
    const diasParaProxima = Math.floor((dataProxima - hoje) / (1000*60*60*24));
    subAval = diasParaProxima > 0
      ? 'Última: há ' + diasPassados + ' dias · próxima sugerida em ' + diasParaProxima + ' dias'
      : 'Última: há ' + diasPassados + ' dias · já está na hora de reavaliar';
  }
  const conteudoAvaliacaoFisica = {
    n: 'Avaliação física', cat: 'Antes de começar', locked: false, video: null,
    desc: 'A avaliação física é o que garante que o treino é montado pra você de verdade, não um genérico. Medidas, composição corporal e desvios posturais entram direto na sua prescrição. O ideal é repetir a cada 90 dias, pra acompanhar sua evolução real.'
  };
  poster(rowAval, 'Avaliação física', subAval, function(){ openDetail('conteudo', conteudoAvaliacaoFisica); });

  renderComposicaoAtual(alunaObj);
  renderMinhaEvolucao(alunaObj);
}

function renderComposicaoAtual(a){
  const elPeso = document.getElementById('stat-peso-atual');
  const elPesoData = document.getElementById('stat-peso-data');
  const elGordura = document.getElementById('stat-gordura-atual');
  const elGorduraData = document.getElementById('stat-gordura-data');
  if(!a || !elPeso) return;

  if(a.composicaoAtual && a.composicaoAtual.peso != null){
    elPeso.textContent = String(a.composicaoAtual.peso).replace('.', ',') + ' kg';
    elPesoData.textContent = a.composicaoAtual.data ? 'Atualizado em ' + a.composicaoAtual.data : 'Atualizado';
  } else {
    elPeso.textContent = '--';
    elPesoData.textContent = 'Ainda não informado';
  }

  if(a.composicaoAtual && a.composicaoAtual.gordura != null){
    elGordura.textContent = String(a.composicaoAtual.gordura).replace('.', ',') + '%';
    elGorduraData.textContent = 'Via avaliação física';
  } else {
    elGordura.textContent = '--';
    elGorduraData.textContent = 'Ainda não informado';
  }
}

// ===== GRÁFICO SIMPLES DE EVOLUÇÃO (peso / gordura ao longo do tempo, sem depender de biblioteca externa) =====
function gerarSvgLinhaEvolucao(pontos, largura, altura, cor){
  if(pontos.length < 2) return ''; // precisa de pelo menos 2 pontos pra desenhar uma linha
  const min = Math.min.apply(null, pontos), max = Math.max.apply(null, pontos);
  const faixa = (max - min) || 1; // evita divisão por zero quando todos os valores são iguais
  const paddingX = 12, paddingY = 16;
  const areaLargura = largura - paddingX * 2, areaAltura = altura - paddingY * 2;
  const coordenadas = pontos.map(function(v, i){
    const x = paddingX + (pontos.length === 1 ? 0 : (i / (pontos.length - 1)) * areaLargura);
    const y = paddingY + areaAltura - ((v - min) / faixa) * areaAltura;
    return { x: x, y: y };
  });
  const linhaPath = coordenadas.map(function(p, i){ return (i === 0 ? 'M' : 'L') + p.x.toFixed(1) + ',' + p.y.toFixed(1); }).join(' ');
  const pontosCirculos = coordenadas.map(function(p){ return '<circle cx="' + p.x.toFixed(1) + '" cy="' + p.y.toFixed(1) + '" r="2.5" fill="' + cor + '"></circle>'; }).join('');
  return '<svg viewBox="0 0 ' + largura + ' ' + altura + '" width="100%" height="' + altura + '" style="display:block;">' +
    '<path d="' + linhaPath + '" fill="none" stroke="' + cor + '" stroke-width="2"></path>' +
    pontosCirculos +
  '</svg>';
}

function renderMinhaEvolucao(a){
  const container = document.getElementById('area-minha-evolucao');
  if(!container) return;
  const historico = (a && a.composicaoHistorico) || [];
  if(historico.length === 0){
    container.innerHTML = '<p class="txt" style="color:var(--text-faint);">Ainda sem histórico. Assim que você registrar novas informações, os valores anteriores ficam guardados aqui pra comparar.</p>';
    return;
  }

  let html = '';
  const pesos = historico.filter(function(r){ return r.peso != null; }).map(function(r){ return parseFloat(r.peso); });
  const gorduras = historico.filter(function(r){ return r.gordura != null; }).map(function(r){ return parseFloat(r.gordura); });
  const graficoPeso = gerarSvgLinhaEvolucao(pesos, 300, 70, '#E8C58A');
  const graficoGordura = gerarSvgLinhaEvolucao(gorduras, 300, 70, '#F4D9A5');
  if(graficoPeso || graficoGordura){
    html += '<div class="row" style="gap:8px;margin-bottom:10px;">';
    if(graficoPeso) html += '<div class="info-box" style="flex:1;"><p class="lbl" style="font-size:11px;margin-bottom:4px;">Peso (kg)</p>' + graficoPeso + '</div>';
    if(graficoGordura) html += '<div class="info-box" style="flex:1;"><p class="lbl" style="font-size:11px;margin-bottom:4px;">% Gordura</p>' + graficoGordura + '</div>';
    html += '</div>';
  }

  html += historico.slice().reverse().map(function(reg){
    let linha = reg.data || ('Semana ' + reg.semana);
    const partes = [];
    if(reg.peso != null) partes.push(String(reg.peso).replace('.', ',') + ' kg');
    if(reg.gordura != null) partes.push(String(reg.gordura).replace('.', ',') + '% gordura');
    if(reg.massaMagra != null) partes.push(String(reg.massaMagra).replace('.', ',') + ' kg massa magra');
    if(reg.pesoMuscular != null) partes.push(String(reg.pesoMuscular).replace('.', ',') + ' kg peso muscular');
    return '<div class="info-box" style="margin-bottom:8px;"><p class="lbl">' + linha + '</p><p class="txt">' + (partes.join(' · ') || 'sem detalhe') + '</p></div>';
  }).join('');
  container.innerHTML = html;
}

// ===== Fluxo de registro de novas informações de composição =====
let registroComposicaoEmAndamento = null;

function iniciarRegistroComposicaoNova(){
  registroComposicaoEmAndamento = { etapa: 'peso' };
  const container = document.getElementById('area-registro-composicao');
  container.innerHTML = '<div class="info-box" style="margin-top:10px;" id="pergunta-composicao">' +
    '<p class="lbl">Qual o seu peso atual?</p>' +
    '<div class="form-group"><input class="form-input" id="input-composicao-peso" type="number" step="0.1" placeholder="Ex: 62.5"></div>' +
    '<button class="btn-gold" onclick="avancarRegistroComposicao()">Continuar</button>' +
  '</div>';
}

function avancarRegistroComposicao(){
  const estado = registroComposicaoEmAndamento;
  if(!estado) return;
  const container = document.getElementById('pergunta-composicao');

  if(estado.etapa === 'peso'){
    const peso = parseFloat(document.getElementById('input-composicao-peso').value);
    if(isNaN(peso)) return;
    estado.peso = peso;
    estado.etapa = 'avaliacao';
    container.innerHTML = '<p class="lbl">Foi feita alguma avaliação física recentemente?</p>' +
      '<div style="display:flex;gap:8px;margin-top:8px;">' +
        '<span class="chip" style="cursor:pointer;" onclick="responderAvaliacaoFeita(true)">Sim</span>' +
        '<span class="chip" style="cursor:pointer;" onclick="responderAvaliacaoFeita(false)">Não</span>' +
      '</div>';
    return;
  }
}

function responderAvaliacaoFeita(sim){
  const estado = registroComposicaoEmAndamento;
  if(!estado) return;
  const container = document.getElementById('pergunta-composicao');
  if(!sim){
    finalizarRegistroComposicao();
    return;
  }
  estado.etapa = 'gordura';
  container.innerHTML = '<p class="lbl">Qual foi o seu percentual de gordura?</p>' +
    '<div class="form-group"><input class="form-input" id="input-composicao-gordura" type="number" step="0.1" placeholder="Ex: 24.5"></div>' +
    '<button class="btn-gold" onclick="avancarParaMassaMagra()">Continuar</button>';
}

function avancarParaMassaMagra(){
  const estado = registroComposicaoEmAndamento;
  const gorduraVal = parseFloat(document.getElementById('input-composicao-gordura').value);
  if(!isNaN(gorduraVal)) estado.gordura = gorduraVal;
  const container = document.getElementById('pergunta-composicao');
  container.innerHTML = '<p class="lbl">Qual foi a sua massa magra? (tudo menos gordura, em kg)</p>' +
    '<p class="txt" style="font-size:11px;color:var(--text-faint);margin-top:-4px;">Se não souber ou a avaliação não trouxe esse dado, pode pular</p>' +
    '<div class="form-group"><input class="form-input" id="input-composicao-massamagra" type="number" step="0.1" placeholder="Ex: 47.2"></div>' +
    '<div style="display:flex;gap:8px;">' +
      '<button class="btn-gold" style="flex:1;" onclick="avancarParaPesoMuscular()">Continuar</button>' +
      '<button class="btn-gold" style="flex:1;background:var(--card-2);color:var(--gold-soft);border:1px solid var(--border);" onclick="pularEIrParaPesoMuscular()">Pular</button>' +
    '</div>';
}

function pularEIrParaPesoMuscular(){
  avancarParaPesoMuscular(true);
}

function avancarParaPesoMuscular(pulou){
  const estado = registroComposicaoEmAndamento;
  if(!pulou){
    const massaVal = parseFloat(document.getElementById('input-composicao-massamagra').value);
    if(!isNaN(massaVal)) estado.massaMagra = massaVal;
  }
  const container = document.getElementById('pergunta-composicao');
  container.innerHTML = '<p class="lbl">E o peso muscular específico? (em kg, aparece nas avaliações antropométricas)</p>' +
    '<p class="txt" style="font-size:11px;color:var(--text-faint);margin-top:-4px;">Diferente de massa magra — se não souber, pode pular</p>' +
    '<div class="form-group"><input class="form-input" id="input-composicao-pesomuscular" type="number" step="0.1" placeholder="Ex: 24.8"></div>' +
    '<div style="display:flex;gap:8px;">' +
      '<button class="btn-gold" style="flex:1;" onclick="finalizarComGorduraEMassa()">Salvar</button>' +
      '<button class="btn-gold" style="flex:1;background:var(--card-2);color:var(--gold-soft);border:1px solid var(--border);" onclick="finalizarRegistroComposicao()">Pular e salvar</button>' +
    '</div>';
}

function finalizarComGorduraEMassa(){
  const estado = registroComposicaoEmAndamento;
  const pesoMuscularVal = parseFloat(document.getElementById('input-composicao-pesomuscular').value);
  if(!isNaN(pesoMuscularVal)) estado.pesoMuscular = pesoMuscularVal;
  finalizarRegistroComposicao();
}

function finalizarRegistroComposicao(){
  const estado = registroComposicaoEmAndamento;
  if(!estado) return;
  const a = alunasPersonal.find(function(x){ return x.nome === NOME_ALUNA_LOGADA; });
  if(!a) return;
  const prog = getProgressoAluna(a.nome);

  // Joga o valor ANTIGO pro histórico antes de sobrescrever, pra nunca perder a evolução
  if(!a.composicaoHistorico) a.composicaoHistorico = [];
  if(a.composicaoAtual && (a.composicaoAtual.peso != null || a.composicaoAtual.gordura != null)){
    a.composicaoHistorico.push(Object.assign({ semana: prog.semana }, a.composicaoAtual));
  }

  const hoje = new Date();
  a.composicaoAtual = {
    peso: estado.peso != null ? estado.peso : (a.composicaoAtual ? a.composicaoAtual.peso : null),
    gordura: estado.gordura != null ? estado.gordura : (a.composicaoAtual ? a.composicaoAtual.gordura : null),
    massaMagra: estado.massaMagra != null ? estado.massaMagra : (a.composicaoAtual ? a.composicaoAtual.massaMagra : null),
    pesoMuscular: estado.pesoMuscular != null ? estado.pesoMuscular : (a.composicaoAtual ? a.composicaoAtual.pesoMuscular : null),
    data: hoje.toLocaleDateString('pt-BR')
  };

  // Cruza com o resto do sistema: mantém a.peso/a.pesoAtual atualizados, usados no cálculo de IMC e DNA
  a.pesoAtual = estado.peso;
  if(!a.pesoHistorico) a.pesoHistorico = [];
  a.pesoHistorico.push({ semana: prog.semana, peso: estado.peso });

  salvarPerfilAlunaNoSupabase(a.nome);

  registroComposicaoEmAndamento = null;
  const container = document.getElementById('area-registro-composicao');
  container.innerHTML = '<p class="txt" style="color:var(--success);">Registrado! Isso já entra nos seus cálculos de composição e evolução.</p>';
  renderComposicaoAtual(a);
  renderMinhaEvolucao(a);
}

const rowSuporte = document.getElementById('row-suporte');
poster(rowSuporte, 'Execução: agachamento livre', '4 min', function(){ openDetail('video'); });
poster(rowSuporte, 'Como ajustar sua carga', '3 min', function(){ openDetail('video'); });
poster(rowSuporte, 'Fale com seu personal', 'Suporte direto', function(){ openDetail('video'); });

const rowDicas = document.getElementById('row-dicas');
poster(rowDicas, 'Amplitude no leg press', '2 min', function(){ openDetail('video'); });
poster(rowDicas, 'Postura no agachamento', '2 min', function(){ openDetail('video'); });

const conteudos = [
  {n:'Plano Nutricional MUSA+', cat:'Nutrição', locked:true, preco:'R$ 97/mês', desc:'Plano alimentar prescrito, individualizado, integrado ao seu DNA MUSA. Em breve, pagamentos ainda não estão ativos na plataforma.'},
  {n:'Fundamentos da Hipertrofia Feminina', cat:'Cursos completos', locked:false, desc:'Curso introdutório com os princípios de treino que guiam toda a metodologia MUSA+. 12 aulas · 3h40.'},
  {n:'Protocolo Avançado de Glúteos', cat:'Cursos completos', locked:true, desc:'Aprofundamento em métodos de intensidade e periodização para ênfase de glúteo. 18 aulas · 5h10.'},
  {n:'Correção Postural na Prática', cat:'Cursos completos', locked:true, desc:'Como identificar e corrigir os principais desvios posturais no dia a dia de treino. 9 aulas · 2h20.'},
  {n:'Introdução ao método MUSA+', cat:'Introdução', locked:false, desc:'Uma visão geral de como a plataforma e a metodologia funcionam juntas. 6 min.'},
  {n:'Como interpretar seu DNA', cat:'Introdução', locked:false, desc:'Entenda o que cada indicador do seu DNA MUSA significa na prática. 5 min.'},
  {n:'Cluster set e restpause na prática', cat:'Métodos de treino', locked:false, desc:'Como executar corretamente os métodos de intensidade usados no seu treino. 4 min.'},
  {n:'Rest Pause', cat:'Métodos de treino', locked:false, desc:'Como aplicar o método restpause corretamente durante o treino.', video:'https://youtu.be/ZpojZRSlNN0'},
  {n:'Drop-Set', cat:'Métodos de treino', locked:false, desc:'Entenda o método dropset e quando utilizá-lo.', video:'https://youtu.be/q07XMVmZ1qk'},
  {n:'Bi-Set', cat:'Métodos de treino', locked:false, desc:'Como estruturar um bi-set de forma eficiente.', video:'https://youtu.be/rSEVm1h0XtA'},
  {n:'Tri-Set', cat:'Métodos de treino', locked:false, desc:'Como estruturar um tri-set de forma eficiente.', video:'https://youtu.be/-YDqVqmsUg8'},
  {n:'Pirâmide Descrescente', cat:'Métodos de treino', locked:false, desc:'Método de pirâmide decrescente explicado na prática.', video:'https://youtu.be/Q1Prp4AE5EM'},
  {n:'Pirâmide Crescente', cat:'Métodos de treino', locked:false, desc:'Método de pirâmide crescente explicado na prática.', video:'https://youtu.be/2lnzLHARO-Y'},
  {n:'Método Circuito', cat:'Métodos de treino', locked:false, desc:'Como montar e executar um treino em circuito.', video:'https://youtu.be/cXBh3UR03SA'},
  {n:'Método Gvt', cat:'Métodos de treino', locked:false, desc:'Entenda o método GVT (German Volume Training).', video:'https://youtu.be/JoA6V-KF5hY'},
  {n:'Ajuste De Cargas', cat:'Métodos de treino', locked:false, desc:'Como ajustar cargas de forma inteligente ao longo do treino.', video:'https://youtu.be/sHNdfqr6yAQ'},
  {n:'Ajuste De Cargas', cat:'Métodos de treino', locked:false, desc:'Como ajustar cargas de forma inteligente ao longo do treino.', video:'https://youtu.be/sHNdfqr6yAQ'},
  {n:'Execução: agachamento livre', cat:'Exercícios de coxa', locked:false, desc:'Guia completo de execução, amplitude e posicionamento correto. 4 min.'},
  {n:'Execução: cadeira extensora', cat:'Exercícios de coxa', locked:false, desc:'Detalhes técnicos para maximizar o estímulo de quadríceps com segurança. 3 min.'},
  {n:'Execução: elevação pélvica', cat:'Exercícios de glúteo', locked:false, desc:'Como posicionar o quadril e manter a contração correta durante o movimento. 3 min.'},
  {n:'Execução: abdução na polia', cat:'Exercícios de glúteo', locked:false, desc:'Técnica para isolar o glúteo médio com eficiência. 2 min.'},
  {n:'Manobras Respiratórias, Curso Completo', cat:'Cursos completos', locked:false, desc:'Sequência completa: 3 vídeos de introdução (respiração, liberação de fáscia, manobra respiratória) + 4 aulas práticas.', aulas:[
    {titulo:'Parte 1: Aprenda a Respirar', video:'https://youtu.be/8ixgAPGj2nE'},
    {titulo:'Parte 2: Liberação da Fáscia Muscular', video:'https://youtu.be/wdgyIXsuoG8'},
    {titulo:'Parte 3: Manobra Respiratória', video:'https://youtu.be/xFU5MbneT9E'},
    {titulo:'Aula 1', video:'https://youtu.be/1jbwkF3qvlY'},
    {titulo:'Aula 2', video:'https://youtu.be/zTavlpabQE0'},
    {titulo:'Aula 3', video:'https://youtu.be/UjsuO_E2HWI'},
    {titulo:'Aula 4', video:'https://youtu.be/AMS6nbcyk0k'}
  ]}
,
  {n:'Alongamento Ilio Psoas', cat:'Alongamentos', locked:false, desc:'', video:'https://www.youtube.com/watch?v=O16dSE_WSFM&sttick=0'},
  {n:'Alongamento Ilio Psoas', cat:'Alongamentos', locked:false, desc:'', video:'https://www.youtube.com/watch?v=O16dSE_WSFM'},
  {n:'Alongamento Quadrado Lombar', cat:'Alongamentos', locked:false, desc:'', video:'https://www.youtube.com/watch?v=It8Z4VV037E'},
  {n:'Alongamento Adutores 4 apoios', cat:'Alongamentos', locked:false, desc:'', video:'https://www.youtube.com/watch?v=YboGYC5A0m4'},
  {n:'Alongamento de adutores no espaldar', cat:'Alongamentos', locked:false, desc:'', video:'https://www.youtube.com/watch?v=BBj2jjsuyD8'},
  {n:'Alongamento dinâmico para adutores', cat:'Alongamentos', locked:false, desc:'', video:'https://www.youtube.com/watch?v=pkgH1kU3t2E'},
  {n:'Alongamento adutores em 4 apoios', cat:'Alongamentos', locked:false, desc:'', video:'https://www.youtube.com/watch?v=fmJeUB5prcE'},
  {n:'Alongamento de Quadrado lombar', cat:'Alongamentos', locked:false, desc:'', video:'https://www.youtube.com/watch?v=9-n3D4vCy0k'},
  {n:'Alongamento reto abdômen', cat:'Alongamentos', locked:false, desc:'', video:'https://www.youtube.com/shorts/uw2lrqLXv-I'},
  {n:'Mob Quadril - Retração', cat:'Mobilidades', locked:false, desc:'', video:'https://www.youtube.com/watch?v=Yb8-QOlWScM&sttick=0'},
  {n:'Wall Ball Slide', cat:'Mobilidades', locked:false, desc:'', video:'https://www.youtube.com/watch?v=SBaDnfc9SgE'},
  {n:'Mob.Ombros Deitado', cat:'Mobilidades', locked:false, desc:'', video:'https://www.youtube.com/watch?v=HwDBulMMpcE'},
  {n:'Flexibilidade, mobilidade, inibição e ativação muscular, e estratégia matadora!!!', cat:'Mobilidades', locked:false, desc:'', video:'https://www.youtube.com/watch?v=Ow2ty_kr0rg'},
  {n:'Mobilidade de ombros deitado', cat:'Mobilidades', locked:false, desc:'', video:'https://www.youtube.com/watch?v=wVq6N153phA'},
  {n:'Mobilidade de ombros deitado', cat:'Mobilidades', locked:false, desc:'', video:'https://www.youtube.com/watch?v=wVq6N153phA'},
  {n:'Mobilidade De quadril em 4 apoios', cat:'Mobilidades', locked:false, desc:'', video:'https://www.youtube.com/watch?v=8K9tohyJpys'},
  {n:'Mobilidade De tornozelo', cat:'Mobilidades', locked:false, desc:'', video:'https://www.youtube.com/watch?v=0LY22_SNW-I'},
  {n:'Mobilidade de quadril - ílio psoas', cat:'Mobilidades', locked:false, desc:'', video:'https://www.youtube.com/watch?v=nJhjDIKlNzo'},
  {n:'MOBILIDADE QUADRIL -EXTENSÃO DIAGONAL', cat:'Mobilidades', locked:false, desc:'', video:'https://www.youtube.com/shorts/t-xiIUGzO3s'},
  {n:'Mobilidade glúteo médio e máximo', cat:'Mobilidades', locked:false, desc:'', video:'https://www.youtube.com/shorts/H_OAHdhD2Ho'},
  {n:'Mobilidade tensor da fascia lata', cat:'Mobilidades', locked:false, desc:'', video:'https://www.youtube.com/shorts/HAXkyuefy2c'},
  {n:'Mobilidade isquiotibiais', cat:'Mobilidades', locked:false, desc:'', video:'https://www.youtube.com/shorts/lmtoMuWbuw4'},
  {n:'Manobra respiratória - Deitado', cat:'Manobras respiratórias', locked:false, desc:'', video:'https://www.youtube.com/watch?v=7YOwjazURoc&sttick=0'},
  {n:'Manobra respiratória - 4 apoios', cat:'Manobras respiratórias', locked:false, desc:'', video:'https://www.youtube.com/watch?v=vv_FhqcCkus&sttick=0'},
  {n:'Manobra respiratória - 4 apoios', cat:'Manobras respiratórias', locked:false, desc:'', video:'https://www.youtube.com/watch?v=vv_FhqcCkus&sttick=0'},
  {n:'Introdução Manobras respiratórias parte 1- Aprenda a respirar', cat:'Manobras respiratórias', locked:false, desc:'', video:'https://www.youtube.com/watch?v=8ixgAPGj2nE'},
  {n:'Introdução Manobras respiratórias parte 2 - Liberação da fáscia muscular', cat:'Manobras respiratórias', locked:false, desc:'', video:'https://www.youtube.com/watch?v=wdgyIXsuoG8'},
  {n:'Introdução Manobras respiratórias parte 3- Manobra Respiratória', cat:'Manobras respiratórias', locked:false, desc:'', video:'https://www.youtube.com/watch?v=xFU5MbneT9E'},
  {n:'Introdução Manobras respiratórias parte 3- Manobra Respiratória', cat:'Manobras respiratórias', locked:false, desc:'', video:'https://www.youtube.com/watch?v=xFU5MbneT9E'},
  {n:'Manobras respiratórias -Aula 2', cat:'Manobras respiratórias', locked:false, desc:'', video:'https://www.youtube.com/watch?v=zTavlpabQE0'},
  {n:'Manobras respiratórias - Aula 3', cat:'Manobras respiratórias', locked:false, desc:'', video:'https://www.youtube.com/watch?v=UjsuO_E2HWI'},
  {n:'Manobras respiratórias - Aula 4', cat:'Manobras respiratórias', locked:false, desc:'', video:'https://www.youtube.com/watch?v=AMS6nbcyk0k'},
  {n:'Potência - Hack', cat:'Ajustes e orientações', locked:false, desc:'', video:'https://www.youtube.com/watch?v=tY21tc4b-Ws'},
  {n:'Puxada Aberta - Entenda', cat:'Ajustes e orientações', locked:false, desc:'', video:'https://www.youtube.com/watch?v=jRhXwHjK45w'},
  {n:'Posição dos pés no Leg', cat:'Ajustes e orientações', locked:false, desc:'', video:'https://www.youtube.com/watch?v=mOOJlKUEkeg'},
  {n:'Leg Press 45 Yes ENTENDA', cat:'Ajustes e orientações', locked:false, desc:'', video:'https://www.youtube.com/watch?v=9KmOJzQU8dk'},
  {n:'Feedback Elevação Lateral (flexão de cotovelo)', cat:'Ajustes e orientações', locked:false, desc:'', video:'https://www.youtube.com/watch?v=-vL07cUk4Sw'},
  {n:'Entenda o Stiff, erros comuns e correções!!', cat:'Ajustes e orientações', locked:false, desc:'', video:'https://www.youtube.com/watch?v=VQstxP0ZN7A'},
  {n:'Abdução na polia erros e ajustes', cat:'Ajustes e orientações', locked:false, desc:'', video:'https://www.youtube.com/watch?v=scncHmltD-c'},
  {n:'Ajustes Pulldown com barra', cat:'Ajustes e orientações', locked:false, desc:'', video:'https://www.youtube.com/watch?v=IUJafieBWEk'},
  {n:'Feedback PUXADA ABERTA', cat:'Ajustes e orientações', locked:false, desc:'', video:'https://www.youtube.com/watch?v=sEyjNuu6VtM'},
  {n:'Feedback REMADA BAIXA', cat:'Ajustes e orientações', locked:false, desc:'', video:'https://www.youtube.com/watch?v=MUSpNCV9q8E'},
  {n:'Feedback Elevação Lateral (flexão de cotovelo)', cat:'Ajustes e orientações', locked:false, desc:'', video:'https://www.youtube.com/watch?v=-vL07cUk4Sw'},
  {n:'Ajustes e métodos que vão POTENCIALIZAR os seus resultados de GLÚTEOS', cat:'Ajustes e orientações', locked:false, desc:'', video:'https://www.youtube.com/watch?v=xBoTRn0MFs4'},
  {n:'Ajustes e métodos que vão POTENCIALIZAR os seus resultados de GLÚTEOS', cat:'Ajustes e orientações', locked:false, desc:'', video:'https://www.youtube.com/watch?v=xBoTRn0MFs4'},
  {n:'Ajustes e métodos que vai potencializar seus ganhos de glúteos - Parte 2', cat:'Ajustes e orientações', locked:false, desc:'', video:'https://www.youtube.com/watch?v=vL9j_bUQLXI'},
  {n:'Ajustes Pulldown com barra', cat:'Ajustes e orientações', locked:false, desc:'', video:'https://www.youtube.com/watch?v=IUJafieBWEk'},
  {n:'ENTENDA O SEU MATERIAL DE AERÓBICOS', cat:'Ajustes e orientações', locked:false, desc:'', video:'https://www.youtube.com/watch?v=fw1rFDDLVe4'},
  {n:'Ajustes para puxada com triângulo', cat:'Ajustes e orientações', locked:false, desc:'', video:'https://www.youtube.com/watch?v=lvPyMBpopZg'},
  {n:'Ajustes para puxada com triângulo', cat:'Ajustes e orientações', locked:false, desc:'', video:'https://www.youtube.com/watch?v=lvPyMBpopZg'},
  {n:'Deixe o seu bumbum REDONDINHO com esse ajuste!!! #academia #gluteos #musculação', cat:'Ajustes e orientações', locked:false, desc:'', video:'https://www.youtube.com/shorts/0G7v5eMpkQk'},
  {n:'AJUSTE PARA O SEU AGACHAMENTO!! #mulheres #musculação', cat:'Ajustes e orientações', locked:false, desc:'', video:'https://www.youtube.com/shorts/DOCVUmGNw8Y'},
  {n:'AJUSTES PARA BOMBAR OS SEUS GLÚTEOS !!🍑', cat:'Ajustes e orientações', locked:false, desc:'', video:'https://www.youtube.com/shorts/jcqUV372G-o'},
  {n:'AJUSTES PARA BOMBAR OS SEUS GLÚTEOS !!🍑', cat:'Ajustes e orientações', locked:false, desc:'', video:'https://www.youtube.com/shorts/jcqUV372G-o'},
  {n:'MÉTODO TRI-SET NA PRÁTICA', cat:'Métodos de treino', locked:false, desc:'', video:'https://www.youtube.com/watch?v=5TpQFCBieDc'},
  {n:'MÉTODO BI-SET NA PRÁTICA', cat:'Métodos de treino', locked:false, desc:'', video:'https://www.youtube.com/watch?v=Hpo-LSoO3Mk'},
  {n:'MÉTODO TRI-SET NA PRÁTICA', cat:'Métodos de treino', locked:false, desc:'', video:'https://www.youtube.com/watch?v=XKJn6LO2czE'},
  {n:'Método circuito', cat:'Métodos de treino', locked:false, desc:'', video:'https://www.youtube.com/watch?v=cXBh3UR03SA'},
  {n:'Método Pirâmide Crescente', cat:'Métodos de treino', locked:false, desc:'', video:'https://www.youtube.com/watch?v=2lnzLHARO-Y'},
  {n:'Método Pirâmide Decrescente', cat:'Métodos de treino', locked:false, desc:'', video:'https://www.youtube.com/watch?v=Q1Prp4AE5EM'},
  {n:'Método tri-set', cat:'Métodos de treino', locked:false, desc:'', video:'https://www.youtube.com/watch?v=-YDqVqmsUg8'},
  {n:'Método Bi-Set', cat:'Métodos de treino', locked:false, desc:'', video:'https://www.youtube.com/'},
  {n:'Método Drop-Set', cat:'Métodos de treino', locked:false, desc:'', video:'https://www.youtube.com/watch?v=q07XMVmZ1qk'},
  {n:'Método Pirâmide Crescente', cat:'Métodos de treino', locked:false, desc:'', video:'https://www.youtube.com/watch?v=2lnzLHARO-Y'},
  {n:'Método Pirâmide Crescente', cat:'Métodos de treino', locked:false, desc:'', video:'https://www.youtube.com/watch?v=2lnzLHARO-Y'},
  {n:'Métodos mais utilizados para BOMBAR seus glúteos!!', cat:'Métodos de treino', locked:false, desc:'', video:'https://www.youtube.com/watch?v=EKTHS26kPpU'},
  {n:'Emagreça Dormindo!!-Fit30', cat:'Curso: Fit 30', locked:false, desc:'', video:'https://www.youtube.com/watch?v=dSA8ATAKF2U'},
  {n:'Exercícios Multiarticulares -Fit30', cat:'Curso: Fit 30', locked:false, desc:'', video:'https://www.youtube.com/watch?v=Iqp6a7yBHbc'},
  {n:'O que é Falha/Fadiga MUSCULAR - Fit30', cat:'Curso: Fit 30', locked:false, desc:'', video:'https://www.youtube.com/watch?v=xHJVFAqjHrU'},
  {n:'O que você vai sentir nos treinos-Fit30', cat:'Curso: Fit 30', locked:false, desc:'', video:'https://www.youtube.com/watch?v=geyjZ_W6wGo'},
  {n:'Quantas vezes na semana preciso treinar?? e se eu faltar?? -Fit30', cat:'Curso: Fit 30', locked:false, desc:'', video:'https://www.youtube.com/watch?v=2r2tgU5q3Qg'},
  {n:'Como contrair o Abdômen durante os exercícios?? - Fit30', cat:'Curso: Fit 30', locked:false, desc:'', video:'https://www.youtube.com/watch?v=oB4KB7-Z6zM'},
  {n:'Avaliação Física -Fit 30', cat:'Curso: Fit 30', locked:false, desc:'', video:'https://www.youtube.com/watch?v=l3zDPKd8HcA'},
  {n:'Como respirar durante os exercícios? -Fit 30', cat:'Curso: Fit 30', locked:false, desc:'', video:'https://www.youtube.com/watch?v=UJE1CRUEbag'},
  {n:'Suporte e uso da plataforma -Fit30', cat:'Curso: Fit 30', locked:false, desc:'', video:'https://www.youtube.com/watch?v=3JtvXylu9O0'},
  {n:'Devo aquecer ou alongar antes do treino?? Fit-30', cat:'Curso: Fit 30', locked:false, desc:'', video:'https://www.youtube.com/watch?v=6abI0QSpg5Q'},
  {n:'Suporte Corpo de Musa', cat:'Curso: Corpo de Musa', locked:false, desc:'', video:'https://www.youtube.com/watch?v=6GeBeEFuxcw'},
  {n:'Avaliação Física Corpo de Musa', cat:'Curso: Corpo de Musa', locked:false, desc:'', video:'https://www.youtube.com/watch?v=qsW55xOnZZU'},
  {n:'Plano alimentar Corpo de musa', cat:'Curso: Corpo de Musa', locked:false, desc:'', video:'https://www.youtube.com/watch?v=wdoTgzTGAdg'},
  {n:'Balela plano alimentar Corpo de musa', cat:'Curso: Corpo de Musa', locked:false, desc:'', video:'https://www.youtube.com/watch?v=-NwPUIh5jqo'},
  {n:'Fadiga Muscular Corpo de musa', cat:'Curso: Corpo de Musa', locked:false, desc:'', video:'https://www.youtube.com/watch?v=HlkAQ7ibq4c'},
  {n:'O que você vai sentir Corpo de musa', cat:'Curso: Corpo de Musa', locked:false, desc:'', video:'https://www.youtube.com/watch?v=g0zab5B5ifg'},
  {n:'Quantas vezes treinar Corpo de musa', cat:'Curso: Corpo de Musa', locked:false, desc:'', video:'https://www.youtube.com/watch?v=pxT9hQQPrnw'},
  {n:'Porque você não consegue resultados expressivos com o treinamento!!', cat:'Conteúdo geral', locked:false, desc:'', video:'https://www.youtube.com/watch?v=wTOur77mYg0'},
  {n:'Introdução ao Bracing', cat:'Conteúdo geral', locked:false, desc:'', video:'https://www.youtube.com/watch?v=hOISEbyv-bY'},
  {n:'Como ajustar a cadeira extensora e flexora', cat:'Conteúdo geral', locked:false, desc:'', video:'https://www.youtube.com/watch?v=_3wEjxulemk'},
  {n:'Abdução de quadril com caneleira- Variação para fibras posteriores de gmed', cat:'Conteúdo geral', locked:false, desc:'', video:'https://www.youtube.com/watch?v=Qywu4yhMJoo'},
  {n:'Exercícios que não podem faltar no seu treino de glúteos -Parte 1', cat:'Conteúdo geral', locked:false, desc:'', video:'https://www.youtube.com/watch?v=RBq_OBCtK1w'},
  {n:'Exercícios que não podem faltar no seu treino de glúteos -Parte 2', cat:'Conteúdo geral', locked:false, desc:'', video:'https://www.youtube.com/watch?v=GJwkXoLkc08'},
  {n:'Exercícios que não podem faltar no seu treino de glúteos -Parte 3', cat:'Conteúdo geral', locked:false, desc:'', video:'https://www.youtube.com/watch?v=cXz9AiP2-Bo'},
  {n:'COMO AUMENTAR GLÚTEOS E DEIXAR SEU BUMBUM DURINHO', cat:'Conteúdo geral', locked:false, desc:'', video:'https://www.youtube.com/watch?v=RD7gN0IrHJ4'},
  {n:'Bora diminuir os erros a aumentar os acertos!!?', cat:'Conteúdo geral', locked:false, desc:'', video:'https://www.youtube.com/watch?v=EIaWDgiwURM'},
  {n:'3 exercícios que vão transformar o seu bumbum PP em um bumbum GG', cat:'Conteúdo geral', locked:false, desc:'', video:'https://www.youtube.com/watch?v=vkpifTgG_lI'},
  {n:'AGACHAMENTO OU CANELEIRAS PARA GLÚTEOS? Qual é o melhor?', cat:'Conteúdo geral', locked:false, desc:'', video:'https://www.youtube.com/watch?v=gtVli4ygY6c&sttick=0'},
  {n:'3 exercícios que vão transformar o seu bumbum PP em um bumbum GG', cat:'Conteúdo geral', locked:false, desc:'', video:'https://www.youtube.com/watch?v=vkpifTgG_lI'},
  {n:'Conteúdo do canal Studio', cat:'Conteúdo geral', locked:false, desc:'', video:'https://studio.youtube.com/channel/UCijPQX5Rjqkoi_hCAzLIf7g/videos/short?filter=%5B%5D&sort=%7B%22columnType%22%3A%22date%22%2C%22sortOrder%22%3A%22DESCENDING%22%7D'},
  {n:'PARA GLÚTEOS MAIS VOLUMOSOS FAÇA ISSO!! 🍑🔥🔥🔥😳#teamthiagofernandes', cat:'Conteúdo geral', locked:false, desc:'', video:'https://www.youtube.com/shorts/8_w8niILq8Q'},
  {n:'#gluteospoderosos', cat:'Conteúdo geral', locked:false, desc:'', video:'https://www.youtube.com/shorts/8_w8niILq8Q'},
  {n:'PARA GLÚTEOS DE RESPEITO FAÇA ISSO!! #academia #personaltrainer #musculação', cat:'Conteúdo geral', locked:false, desc:'', video:'https://www.youtube.com/shorts/vQycL-uH9pA'},
  {n:'#treinofeminino', cat:'Conteúdo geral', locked:false, desc:'', video:'https://www.youtube.com/shorts/vQycL-uH9pA'},
  {n:'3 EXERCÍCIOS PODEROSOS PARA DEIXAR AS COSTAS BONITINHAS PARA USAR', cat:'Conteúdo geral', locked:false, desc:'', video:'https://www.youtube.com/shorts/iwxbMc_YaC4'},
  {n:'BIQUÍNI NA PRAIA!', cat:'Conteúdo geral', locked:false, desc:'', video:'https://www.youtube.com/shorts/iwxbMc_YaC4'},
  {n:'FAÇA ISSO E GANHE ATÉ 10cm DE GLÚTEOS !! #academia #gluteos #musculação', cat:'Conteúdo geral', locked:false, desc:'', video:'https://www.youtube.com/shorts/XuXB82PcZIc'},
  {n:'#teamthiagofernandes', cat:'Conteúdo geral', locked:false, desc:'', video:'https://www.youtube.com/shorts/XuXB82PcZIc'},
  {n:'#gluteosgrandes', cat:'Conteúdo geral', locked:false, desc:'', video:'https://www.youtube.com/shorts/0G7v5eMpkQk'},
  {n:'FAÇA ISSO E GANHE ATÉ 10cm DE GLÚTEOS !! #academia #gluteos #musculação', cat:'Conteúdo geral', locked:false, desc:'', video:'https://www.youtube.com/shorts/XuXB82PcZIc'},
  {n:'#teamthiagofernandes', cat:'Conteúdo geral', locked:false, desc:'', video:'https://www.youtube.com/shorts/XuXB82PcZIc'},
  {n:'Extensão de quadril na polia com tronco inclinado', cat:'Conteúdo geral', locked:false, desc:'', video:'https://www.youtube.com/shorts/_Z19zMD6ams'},
  {n:'Glúteos poderosos com Step up!! Para mais dicas para, deixa o seu like e se inscreva no', cat:'Conteúdo geral', locked:false, desc:'', video:'https://www.youtube.com/shorts/w2gHltMhrFc'},
  {n:'canal!💪🏼', cat:'Conteúdo geral', locked:false, desc:'', video:'https://www.youtube.com/shorts/w2gHltMhrFc'},
  {n:'DICA INFALÍVEL PARA AUMENTAR O SEU BUMBUM!', cat:'Conteúdo geral', locked:false, desc:'', video:'https://www.youtube.com/shorts/OjeWNdikcSA'},
  {n:'#personalthiagofernandes', cat:'Conteúdo geral', locked:false, desc:'', video:'https://www.youtube.com/shorts/DOCVUmGNw8Y'},
  {n:'QUANDO USAR STEP NO SEU TREINO???', cat:'Conteúdo geral', locked:false, desc:'', video:'https://www.youtube.com/shorts/bxh1v8Hmctc'},
  {n:'Turbine os seus posteriores de coxas!! #fitness #musculação #definiçãomuscular', cat:'Conteúdo geral', locked:false, desc:'', video:'https://www.youtube.com/shorts/9ziP_10wW2U'},
  {n:'#academia', cat:'Conteúdo geral', locked:false, desc:'', video:'https://www.youtube.com/shorts/9ziP_10wW2U'},
  {n:'todos! TOP 25 MELHORES CELULARES para COMPRAR no FIM DO ANO! *Não', cat:'Conteúdo geral', locked:false, desc:'', video:'https://www.youtube.com/watch?v=7pjUy5TE6u4'}
];

const categorias = ['Todos', 'Cursos completos', 'Introdução', 'Métodos de treino', 'Exercícios de coxa', 'Exercícios de glúteo', 'Nutrição', 'Alongamentos', 'Mobilidades', 'Manobras respiratórias', 'Ajustes e orientações', 'Curso: Fit 30', 'Curso: Corpo de Musa', 'Curso: Seca barriga', 'Conteúdo geral'];
let filtroAtivo = 'Todos';

const filterRow = document.getElementById('filter-row');
categorias.forEach(function(cat){
  const chip = document.createElement('div');
  chip.className = 'chip' + (cat === filtroAtivo ? ' active' : '');
  chip.textContent = cat;
  chip.onclick = function(){
    filtroAtivo = cat;
    document.querySelectorAll('.chip').forEach(function(c){ c.classList.remove('active'); });
    chip.classList.add('active');
    renderGrid();
  };
  filterRow.appendChild(chip);
});

function alunaTemAcessoAoConteudo(c){
  if(!c.exclusivoDeCurso) return true; // conteúdo solto normal, sempre visível
  const cursoDoItem = cursosPersonal.find(function(curso){
    return (curso.itensIds || []).some(function(idx){ return conteudos[idx] === c; });
  });
  if(!cursoDoItem) return true; // exclusivo mas sem curso vinculado ainda, não trava por engano
  if(cursoDoItem.aberto) return true;
  return (cursoDoItem.alunasLiberadas || []).indexOf(NOME_ALUNA_LOGADA) !== -1;
}

function renderGrid(){
  const grid = document.getElementById('poster-grid');
  grid.innerHTML = '';
  const lista = conteudos.filter(function(c){
    if(filtroAtivo !== 'Todos' && c.cat !== filtroAtivo) return false;
    if(sessaoTipo === 'aluna' && !alunaTemAcessoAoConteudo(c)) return false;
    return true;
  });
  lista.forEach(function(c){
    const el = document.createElement('div');
    el.className = 'vposter';
    el.innerHTML =
      '<div class="vcover">' +
        (c.locked ? '<div class="lock-icon"><i class="ti ti-lock"></i></div>' : '') +
        '<i class="ti ti-player-play play"></i>' +
      '</div>' +
      '<p class="vtitle">' + c.n + '</p>' +
      (c.preco ? '<p style="font-size:11px;color:var(--gold-soft);margin:2px 0 0;">' + c.preco + '</p>' : '');
    el.onclick = function(){ openDetail('conteudo', c); };
    grid.appendChild(el);
  });
}
renderGrid();

/* ===== PERSONAL ===== */

const alunasPersonal = [
  {nome:'Michelle Cristiane Ferreira', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'A definir', email:'', telefone:'00000000000', piramide:'Abdômen, pernas, cintura e braços', objetivo:'Perder peso e definir', restricoes:'Na adolescência fraturei Clavícula e tornozelo direito.', academia:'', dataAnamnese:'2022-03-09'},
  {nome:'Luana Lenz', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'A definir', email:'', telefone:'00000000000', piramide:'1 Barriga, 2 glúteos, 3 coxas, 4 costas', objetivo:'Emagrecer e definir', restricoes:'Nenhuma relatada', academia:'', dataAnamnese:'2022-03-09'},
  {nome:'Evelline Lindenau', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'', telefone:'00000000000', piramide:'1° glúteos 2° coxas/pernas 3° abdômen 4° superiores', objetivo:'Ter um treino acertivo, manter a constância e assim alcançar resultados', restricoes:'Nenhuma relatada', academia:'Yes', dataAnamnese:'2022-03-14'},
  {nome:'Leticia graff', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'06x por semana', email:'', telefone:'00000000000', piramide:'Todos', objetivo:'Emagrecer', restricoes:'Nenhuma relatada', academia:'Yes', dataAnamnese:'2022-03-14'},
  {nome:'Marelize Geruza dos Santos Brites', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'6x por semana', email:'', telefone:'00000000000', piramide:'1 glúteo, 2 coxas, 3 trícrps, 4 panturrilha', objetivo:'Hipertrofia.', restricoes:'Uma pequena dor no joelho, mas era por fazer o exercicíos errado.', academia:'Atitude fit.', dataAnamnese:'2022-03-14'},
  {nome:'Givanildo de castro Trindade', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'6x por semana', email:'', telefone:'00000000000', piramide:'1=costas  2=peito  3=braço  4=pernas', objetivo:'Hipertrofia', restricoes:'Nenhuma relatada', academia:'Daidojuku', dataAnamnese:'2022-03-14'},
  {nome:'Fabiana Peres', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'', telefone:'00000000000', piramide:'1 bumbum 2 meio da coxa  3 peito 4 costa', objetivo:'Melhorar minha saúde e melhorar o corpo', restricoes:'Bursite dói muito kkkk', academia:'Daido juku', dataAnamnese:'2022-03-14'},
  {nome:'Gabriela Souza Duarte', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'', telefone:'00000000000', piramide:'1:glúteos 2: pernas 3:Costa 4:abdômen', objetivo:'Para me motivar, me manter no foco kkk, me oferecer orientação física, alcança o corpo ideal', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2022-03-15'},
  {nome:'Debora Mann', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'', telefone:'00000000000', piramide:'1Glúteo  2Perna 3Braço 4Abdômen', objetivo:'Emagrecer, tonificar/definir', restricoes:'Nenhuma relatada', academia:'Em Sapucaia', dataAnamnese:'2022-03-15'},
  {nome:'Patrícia Silva', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'', telefone:'00000000000', piramide:'Pernas Braços  Glúteos  Abdômen', objetivo:'Definir e ganhar massa', restricoes:'Nenhuma relatada', academia:'Yes Fitness', dataAnamnese:'2022-03-15'},
  {nome:'Micheli Araujo', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'6x por semana', email:'', telefone:'00000000000', piramide:'1- glúteos  2-posterior 3-quadriceps 4-costas', objetivo:'Emagrecer e definir', restricoes:'Não.', academia:'Performance', dataAnamnese:'2022-03-15'},
  {nome:'Tanaiane da Silva Cardoso', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'', telefone:'00000000000', piramide:'Pernas,bumbum,braços e barriga', objetivo:'Conquistar um shape melhor. Não quero crescer,quero definição', restricoes:'Nunca', academia:'Onde eu estiver', dataAnamnese:'2022-03-15'},
  {nome:'Michele Silva dos Santos', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'', telefone:'00000000000', piramide:'Posterior,glúteo,costas,coxas', objetivo:'Emagrecimento e definição', restricoes:'Nenhuma relatada', academia:'Yes fitnes', dataAnamnese:'2022-03-15'},
  {nome:'Jessica Nunes de Lima', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'', telefone:'00000000000', piramide:'1 pernas 2 glúteos 3 costas', objetivo:'Afinar barriga e definir pernas e glúteos', restricoes:'Nenhuma relatada', academia:'Sublime - Alvorada RS', dataAnamnese:'2022-03-15'},
  {nome:'Jenniffer Barbosa de Oliveira', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'4x por semana', email:'', telefone:'00000000000', piramide:'1 glúteo,  2 coxa , 3 barriga', objetivo:'Coxa mais grossa e barriga definida', restricoes:'As vezes  muita dor nos joelhos', academia:'Engenharia do corpo  Alvorada', dataAnamnese:'2022-03-15'},
  {nome:'Dienifer Silveira', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'', telefone:'00000000000', piramide:'1 glúteo  2 posterior  3 parte interna da coxa  4 definir todo o corpo', objetivo:'Focar, definir o corpo com ênfase nos glúteos e posterior, ter um treino personalizado', restricoes:'Nenhuma relatada', academia:'Yes Fitness, a partir de abril performance Fitness', dataAnamnese:'2022-03-15'},
  {nome:'Deivid Rogger Rodrigues Batista', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'', telefone:'00000000000', piramide:'Peito Abdômen  Costa Braço', objetivo:'Ganhar massa muscular e definição', restricoes:'Tornozelo a 10 anos atraz', academia:'Yes fitnes', dataAnamnese:'2022-03-16'},
  {nome:'Gabriela Guimarães Andrade', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'6x por semana', email:'', telefone:'00000000000', piramide:'Glúteo, quadríceps, posterior e costas', objetivo:'Realizar os exercícios corretamente com foco no meu objetivo', restricoes:'Nenhuma relatada', academia:'Performance fitness', dataAnamnese:'2022-03-16'},
  {nome:'Carolaine de Melo Borges', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'', telefone:'00000000000', piramide:'1', objetivo:'Glúteos kkk', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2022-03-19'},
  {nome:'Paloma Alves de Oliveira', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'', telefone:'00000000000', piramide:'1° abdômen, 2° pernas, 3° glúteos, 4°braços', objetivo:'Emagrecimento e definição', restricoes:'Nenhuma relatada', academia:'Engenharia do Corpo Alvorada', dataAnamnese:'2022-03-20'},
  {nome:'Andri', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'A definir', email:'', telefone:'00000000000', piramide:'Coxas Glúteo  Abdômen  Superiores', objetivo:'Emagrecimento e definição', restricoes:'Nenhuma relatada', academia:'Performace', dataAnamnese:'2022-03-21'},
  {nome:'Débora', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'', telefone:'00000000000', piramide:'1 - glúteo  2 - perna 3 - braços  4 - abdômen', objetivo:'Definição', restricoes:'Nenhuma relatada', academia:'Sapucaia', dataAnamnese:'2022-03-22'},
  {nome:'Suane Maciel Vieira', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'6x por semana', email:'', telefone:'00000000000', piramide:'1° Quadriceps, 2° glúteos, 3° Abdomen, 4° superiores', objetivo:'Definição', restricoes:'Sim, ombro', academia:'Brothers em São Leopoldo', dataAnamnese:'2022-03-29'},
  {nome:'Juliana Rodrigues', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'', telefone:'00000000000', piramide:'Pernas e glúteo', objetivo:'Melhorar cada vez mais', restricoes:'Somente dor nas costas', academia:'Torq Cidade Baixa', dataAnamnese:'2022-03-30'},
  {nome:'andreza araujo feck', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'4x por semana', email:'', telefone:'00000000000', piramide:'1 gluteo, 2 perna ,3 abdomen , 4 costas', objetivo:'Ter mais enfase em meus treinos assim podendo ver resultados que não vejo , treinando por conta', restricoes:'Nenhuma relatada', academia:'academia de poa', dataAnamnese:'2022-04-04'},
  {nome:'Katheyn Quintero', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'', telefone:'00000000000', piramide:'Membros inferiores: focando mais nos glúteos Coxas e panturrilhas Membros superiores: ombros, biceps e costas  Sem esquecer o abdômen', objetivo:'Crear massa magra', restricoes:'Tenho os ombros machucados, consigo treinar mas tem alguns exercícios que se me dificulta por exemplo remada alta.', academia:'Smart Fit', dataAnamnese:'2022-04-08'},
  {nome:'Janaína', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'', telefone:'00000000000', piramide:'Glúteos, posterior de coxas, quadriceps  tríceps', objetivo:'Ficar gostosa', restricoes:'Nenhuma relatada', academia:'Engenharia do corpo', dataAnamnese:'2022-04-10'},
  {nome:'Miriam Carolini Lessa Gonçalves', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'', telefone:'00000000000', piramide:'1-abdomem  2- gluteo 3- quadriceps 5- bicips', objetivo:'Me sentir melhor que hoje.', restricoes:'Nunca', academia:'Pretendo começar em casa, no próximo mês academia do Sesc', dataAnamnese:'2022-04-11'},
  {nome:'Ana Paula de Matos Souza', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'', telefone:'00000000000', piramide:'1° glúteo, 2° coxas, 3° posterior e 4° costas', objetivo:'Quero muito melhorar meu físico, perder gordura localizada. E voltar a me sentir bem comigo mesma.', restricoes:'Nunca.', academia:'Smart fit', dataAnamnese:'2022-04-20'},
  {nome:'Maria Aparecida Pinheiro leal', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'4x por semana', email:'', telefone:'00000000000', piramide:'Quadril', objetivo:'Fortalecer a musculatura e emagrecer', restricoes:'Sim', academia:'Performance', dataAnamnese:'2022-04-26'},
  {nome:'Angelica Cezar de Andrade', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'', telefone:'00000000000', piramide:'1. Glúteos 2. Coxa 3. Costas 4. braços', objetivo:'Perda de peso e definição', restricoes:'Nenhuma relatada', academia:'Engenharia do Corpo', dataAnamnese:'2022-04-28'},
  {nome:'Giovana', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'giovanathamy@gmail.com', telefone:'00000000000', piramide:'1', objetivo:'Chegar ao meu objetivo , sem barriga e bumbum na nuca', restricoes:'Sim', academia:'perfomace', dataAnamnese:'2022-06-24'},
  {nome:'Rafaella Trindade', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'rafaellamtrindade@gmail.com', telefone:'00000000000', piramide:'Barriga , bumbum , perna e costas', objetivo:'Emagrecer  fortalecimento muscular q acho q estou perdendo , e sim né fica gostosa kkk', restricoes:'Lesão não q saiba mas sinto a lombar com frequência', academia:'Performance', dataAnamnese:'2022-07-13'},
  {nome:'Gabriela Amador da Silva', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'gabrielaamador284@gmail.com', telefone:'00000000000', piramide:'1/2/3', objetivo:'Me trazer um corpo mais armonico que eu me sinta satisfeita e mega feliz e que eu consiga com seus treinos sair da minha zona de conforto que acho que nunca posso e não vou conseguir', restricoes:'Meu joelho direito tive uma lesão mais isso faz tempo mais ainda dói em alguns exercícios', academia:'Performance fitness', dataAnamnese:'2022-12-06'},
  {nome:'Eduarda Machado Coutinho Gadea', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'dudamachado00@outlook.com', telefone:'00000000000', piramide:'1- barriga 2- coxa 3- bumbum 4- costas', objetivo:'Emagrecer e ganhar hipertrofia', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2023-01-11'},
  {nome:'Atricia bez cardoso', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'atricia.cardoso@gmail.com', telefone:'00000000000', piramide:'1 pernas 2 barriga 3 braços', objetivo:'Definição muscular', restricoes:'Não uso barra nas costas devido a coluna', academia:'Em varias devido gmypass', dataAnamnese:'2023-01-11'},
  {nome:'Sandrine rosa', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'A definir', email:'sandrine.rosa1@gmail.com', telefone:'51991230734', piramide:'1- bumbum 2-pernas 3-barriga 4-costas/ braços', objetivo:'Evoluir e pegar gosto pelo hábito de ir malhar', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2023-05-27'},
  {nome:'Yasmin Flores', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'yasminflores550@gmail.com', telefone:'51984572913', piramide:'1° bumbum 2° coxa 3° barriga  4° costas', objetivo:'Massa muscular e definição', restricoes:'Não tive lesão', academia:'Performance', dataAnamnese:'2023-06-24'},
  {nome:'Tainara', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Intermediário', freq:'5x por semana', email:'tainarafarruda@gmail.com', telefone:'51985406087', piramide:'Barriga Coxa Bunda  Costas', objetivo:'Chegar no objetivo.', restricoes:'Sim.', academia:'Performance', dataAnamnese:'2023-08-01'},
  {nome:'Cláudia correa', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'4x por semana', email:'cld_maria@hotmail.com', telefone:'7742970981', piramide:'1e 2', objetivo:'Defendi', restricoes:'Pescoço', academia:'Planet Fitness', dataAnamnese:'2024-02-28'},
  {nome:'Fernando Kunzler Paiani', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'6x por semana', email:'Fernandokunzler99@gmail.com', telefone:'51999860995', piramide:'1 ombros  2 peito  3 pernas  4 costas', objetivo:'Quero ganhar massa muscular e emagrecer', restricoes:'Nenhuma lesão', academia:'Performance fitness', dataAnamnese:'2024-09-19'},
  {nome:'IGOR AUGUSTO PIACINI', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Intermediário', freq:'A definir', email:'igor_piacini10@hotmail.com', telefone:'51995861984', piramide:'1 peito 2 costas 3 braços 4 pernas', objetivo:'Ganho de massa e emagrecimento', restricoes:'Nenhuma relatada', academia:'Performance fitnnes', dataAnamnese:'2024-11-13'},
  {nome:'Juliana', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'juhvandrade@gmail.com', telefone:'51996328299', piramide:'1- Costas  2- coxa 3- braços 4- bumbum', objetivo:'Melhorar minha saúde física... Resultados físicos', restricoes:'Nenhuma relatada', academia:'Uma em Osório', dataAnamnese:'2025-02-20'},
  {nome:'AnaLucia da rosa', status:'ok', statusLabel:'Ativa recente', nivel:'Avançado', freq:'5x por semana', email:'darosaana799@gmail.com', telefone:'51981862058', piramide:'Glúteo', objetivo:'Ganhar  músculos  e definir', restricoes:'Nenhuma relatada', academia:'Pratique', dataAnamnese:'2026-07-20'},
  {nome:'Mariel Madlene dos Santos', status:'ok', statusLabel:'Ativa recente', nivel:'Avançado', freq:'5x por semana', email:'mariel.dos.santos@hotmail.com', telefone:'51996036844', piramide:'3,1,2,4', objetivo:'Perda de peso e definição muscular.', restricoes:'Não.', academia:'Performance', dataAnamnese:'2026-07-20'},
  {nome:'Tamara Prestes', status:'ok', statusLabel:'Ativa recente', nivel:'Avançado', freq:'3x por semana', email:'tamaraprestes@yahoo.com', telefone:'51999946085', piramide:'1 e 3', objetivo:'Emagrecimento', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2026-07-20'},
  {nome:'Bruna Melissa', status:'ok', statusLabel:'Ativa recente', nivel:'Avançado', freq:'4x por semana', email:'bruna.melissa4@gmail.com', telefone:'51981415219', piramide:'Costas, coxa, glúteos, barriga', objetivo:'Ter suporte profissional', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2026-07-20'},
  {nome:'Ariana', status:'ok', statusLabel:'Ativa recente', nivel:'Avançado', freq:'5x por semana', email:'ariane-ashley@hotmail.com', telefone:'51985981461', piramide:'Bumbum - Costas - Barriga', objetivo:'Melhorar meu desempenho', restricoes:'Nenhuma relatada', academia:'Daido', dataAnamnese:'2026-07-20'},
  {nome:'Danielle', status:'ok', statusLabel:'Ativa recente', nivel:'Avançado', freq:'5x por semana', email:'danny_kressin@yahoo.com.br', telefone:'51983140058', piramide:'Quadríceps/ posterior coxa/ glúteos/costas', objetivo:'Hipertrofia', restricoes:'Tive na lombar', academia:'Moinhos', dataAnamnese:'2026-07-20'},
  {nome:'Adriana', status:'ok', statusLabel:'Ativa recente', nivel:'Avançado', freq:'5x por semana', email:'schnadelbachadriana90@gmail.com', telefone:'51995027512', piramide:'Todos', objetivo:'Ganhar músculo e perder gordura', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2026-07-20'},
  {nome:'Priscila Becker', status:'ok', statusLabel:'Ativa recente', nivel:'Iniciante', freq:'3x por semana', email:'priscilajbecker20@gmail.com', telefone:'51995987375', piramide:'1 bunda 2 quadríceps 3 ombro e costas 4 posteriores 5 braços', objetivo:'Ficar gostosa', restricoes:'Acho q nada grave', academia:'Engenharia do corpo - Petropolis', dataAnamnese:'2026-07-20'},
  {nome:'Eduarda Mentz', status:'ok', statusLabel:'Ativa recente', nivel:'Avançado', freq:'5x por semana', email:'duda.mentz@gmail.com', telefone:'51995105977', piramide:'2,1,3,4', objetivo:'Emagrecimento é definição', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2026-07-20'},
  {nome:'PATRÍCIA PEREIRA DA SILVA', status:'ok', statusLabel:'Ativa recente', nivel:'Avançado', freq:'5x por semana', email:'patricia.silva.pps12@gmail.com', telefone:'51997656571', piramide:'1 - coxa, 2 - Bumbum, 3 - costas, 4- Ombros', objetivo:'Dar seguimento', restricoes:'Nenhuma relatada', academia:'YES', dataAnamnese:'2026-07-20'},
  {nome:'Viviane Moreira', status:'ok', statusLabel:'Ativa recente', nivel:'Avançado', freq:'4x por semana', email:'vi.dacunha@gmail.com', telefone:'51992907258', piramide:'1 2', objetivo:'Ganhar massa', restricoes:'Nenhuma relatada', academia:'Total', dataAnamnese:'2026-07-20'},
  {nome:'Andreli vanessa de campos', status:'ok', statusLabel:'Ativa recente', nivel:'Avançado', freq:'5x por semana', email:'andrelivanessadecampos@gmail.com', telefone:'48988180167', piramide:'3,2,1,4', objetivo:'Emagrecimento', restricoes:'Nenhuma relatada', academia:'Pratique fitnes', dataAnamnese:'2026-07-20'},
  {nome:'Eliane fatima crupinski', status:'ok', statusLabel:'Ativa recente', nivel:'Intermediário', freq:'4x por semana', email:'elianecrupinskiagro@gmail.com', telefone:'51996504863', piramide:'1 barriga 2 cintura 3costas 4 gluteo 5 pernas', objetivo:'Definir corpo', restricoes:'Nenhuma relatada', academia:'Ctperformace', dataAnamnese:'2026-07-20'},
  {nome:'Priscila', status:'ok', statusLabel:'Ativa recente', nivel:'Avançado', freq:'2x por semana', email:'prytchy@hotmail.com', telefone:'51984816358', piramide:'Barriga 1 Bumbum 2  Costas 3 Coxa 4', objetivo:'Emagrecimento', restricoes:'Aim', academia:'26 fit', dataAnamnese:'2026-07-20'},
  {nome:'Renata Lopes', status:'ok', statusLabel:'Ativa recente', nivel:'Avançado', freq:'3x por semana', email:'renatalopes2530@gmail.com', telefone:'51998064208', piramide:'1coxa  2 bumbum 2 barriga  4 costas', objetivo:'Harmonizar mais meu corpo ganhar mais músculos e perder gordura abdominal ( milagres)', restricoes:'Joelho', academia:'Ideia Cohab', dataAnamnese:'2026-07-21'},
  {nome:'Graziele Steffens', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'', telefone:'', piramide:'1- gluteo 2-coxa 3-abdomen 4-braço', objetivo:'perda de peso e definição (me incomoda muito meu braço e a gordura das costas)', restricoes:'Nenhuma relatada', academia:'academia live fit - canoas', dataAnamnese:'2022-04-28'},
  {nome:'Guilherme Barros', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Intermediário', freq:'5x por semana', email:'guilherminhobarros@yahoo.com.br', telefone:'', piramide:'1°', objetivo:'Evoluir meu corpo.', restricoes:'Não.; Não.; Não.', academia:'Performance', dataAnamnese:'2022-05-04'},
  {nome:'Deise', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'deisemolina.l@icloud.com', telefone:'', piramide:'Bumbum, coxas, barriga e braços', objetivo:'Foco e evolução', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2022-05-07'},
  {nome:'Fabiana', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'4x por semana', email:'fabianabalt@gmail.com', telefone:'', piramide:'1- inferiores -glúteo e coxa 2 - core - abdômen e lombar  3 - postural - escapula aladas ombros e peitos fechado  4 - superiores', objetivo:'Orientação adequada quanto a postura dos exercícios;,  organizar os treinos, aeróbicos  e , flexibilidade dentro da minha disponibilidade e de acordo com o meu porte físico e necessidade do corpo, focando no que eu realmente preciso melhorar. Ter alguém p tirar dúvidas e me dizer o q fazer e qdo fazer.', restricoes:'Tenho Bursite/tendinite no Ombro direito, desgaste manguito , e tive cervicalgia e lombar por falta fortalecimento core e pela intensidade de treinos e corrida. Encurtamento músculo pissoas.; Escoliose em s e hiperlordose .; Não .', academia:'Smartfit / academia casa e aeróbico quartel', dataAnamnese:'2022-05-10'},
  {nome:'Ana Paula', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'5x por semana', email:'anapaulamatossouza5@gmail.com', telefone:'', piramide:'1-Glúteo 2-Coxas 3-Posterior 4-costas', objetivo:'Quero ter uma boa relação com o meu corpo. Me sentir bem comigo mesma.', restricoes:'Nunca tive.; Não.; Nunca', academia:'Star fitniss', dataAnamnese:'2022-05-24'},
  {nome:'gabriel', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'3x por semana', email:'andrades.santosg@gmail.com', telefone:'', piramide:'Braço, peito, costas, abdômen', objetivo:'Crescimento e definição muscular', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2022-06-01'},
  {nome:'Andresa Medeiros de Cardoso', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'5x por semana', email:'andresamedeirosc@outlook.com', telefone:'', piramide:'1• Bunda / 2• Perna / 3• Barriga / 4• superiores', objetivo:'Ganhar massa', restricoes:'Tive no pé esquerdo uma batida muito forte , mas não quebrou , porém as vezes sinto um pouco de dor ( bem leve ) nada que prejudique.', academia:'Smart Fit', dataAnamnese:'2022-06-04'},
  {nome:'Cláudia DIEHL Garcia', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'5x por semana', email:'claudinha18diehl@gmail.com.br', telefone:'', piramide:'Glúteo , coxa', objetivo:'Otimizar resultados', restricoes:'Condropatia grau 3; Condropatia grau 3', academia:'Acho q smart fitness', dataAnamnese:'2022-06-05'},
  {nome:'Jéssica Marins', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'jessicamarins.direito@gmail.com', telefone:'', piramide:'1', objetivo:'Ficar em forma e com musculação bem como ter acesso ao material para saber quais máquinas utilizar para ter um bom resultado.', restricoes:'Não.', academia:'Ainda não sei, a princípio performace deve ter todas as máquinas abaixo', dataAnamnese:'2022-06-07'},
  {nome:'Keli barbieri', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'barbierikeli@gmail.com', telefone:'', piramide:'1 abdômen  2 glúteos  3 coxa', objetivo:'Definir, emagrecer e perder a celulite', restricoes:'Nenhuma relatada', academia:'Performance  Fitness Em Eldorado', dataAnamnese:'2022-06-08'},
  {nome:'Charlene', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'chatrinsi@yahoo.com', telefone:'', piramide:'Não sei bem como te responder isso. Meu objetivo é emagrecer, eliminar barriga crescer o bumbum e tonificar o corpo.', objetivo:'Ter um guia para emagrecer e tonificar o corpo com saúde.', restricoes:'Sim no joelho direito e lombar; Tenho os ligamentos do joelho danificado. Posso fazer praticamente tudo desde que com cuidado.', academia:'PureGym', dataAnamnese:'2022-06-09'},
  {nome:'Luciano Costa de Sá', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Intermediário', freq:'5x por semana', email:'lucianocostasa@gmail.com', telefone:'', piramide:'quero 1) peito mas preciso atualmente 2) biceps e tricps 3) costas 4) Perna', objetivo:'Ganhar massa muscular e perder gordura corporal', restricoes:'braco esquerdo fraturado que consolidou com desvio; ja tive braco esquerdo fraturado que consolidou com desvio; braço esquerdo articulação do cotovelo com minima limitação', academia:'Performance', dataAnamnese:'2022-06-15'},
  {nome:'Vagner Moura Magalhães', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'5x por semana', email:'vagnerxitapemirim@gmail.com', telefone:'', piramide:'Peito braço e costa', objetivo:'Definir', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2022-06-22'},
  {nome:'Eduarda lopes', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'6x por semana', email:'dudalopes1999@gmail.com', telefone:'', piramide:'1- barriga; 2- bumbum; 3- costas, 4- coxa.', objetivo:'Ter auxílio nos treinos para obter resultados bons.', restricoes:'Rompimento parcial do ligamento do tornozelo.', academia:'Performance', dataAnamnese:'2022-06-28'},
  {nome:'Henrique', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'henriquegarcia1597@gmail.com', telefone:'', piramide:'Barriga e cintura ( flancos )', objetivo:'Emagrecimento e fortalecimento muscular', restricoes:'Nenhuma relatada', academia:'Smartfit', dataAnamnese:'2022-06-29'},
  {nome:'Ana Cristina', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'anacpdecastro1@gmail.com', telefone:'', piramide:'1-bumbum 2- coxas 3- barriga 4- costas', objetivo:'Eliminar gordura e ganhar músculo.', restricoes:'Não.', academia:'Performance', dataAnamnese:'2022-07-10'},
  {nome:'Arisson motta', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'6x por semana', email:'arissonmotta17@hotmail.com', telefone:'', piramide:'1-braços 2-peito 3- pernas 4-costas', objetivo:'.', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2022-07-10'},
  {nome:'Larissa', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'larissa.rsantanq@gmail.com', telefone:'', piramide:'1', objetivo:'Ter mais resultados', restricoes:'Nenhuma relatada', academia:'Yes fitness', dataAnamnese:'2022-07-12'},
  {nome:'Franciele Freitas dos Santos', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'3x por semana', email:'francielefreitasdosssantos318@gmail.com', telefone:'', piramide:'Pernas, barriga, costas', objetivo:'Ganhar massa muscular, e manter o meu peso', restricoes:'Nenhuma relatada', academia:'Yes', dataAnamnese:'2022-07-13'},
  {nome:'Fernanda Soares Rodrigues', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'2x por semana', email:'fernandasr3@gmail.com', telefone:'', piramide:'Bumbum, coxas, barriga, costas', objetivo:'Ter um treino organizado para perder gordura e ganhar massa muscular. Definir pernas e bumbum, criar mais cintura e deixar o abdômen mais definido. Tb quero a consultoria para não  me lesionar e fazer corretamente os exercícios.', restricoes:'As vezes for na lombar e  cervical.', academia:'Engenharia do corpo', dataAnamnese:'2022-07-13'},
  {nome:'Simone da Rosa', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Intermediário', freq:'3x por semana', email:'Mony_darosa@hotmail.com', telefone:'', piramide:'1-3-2-4', objetivo:'Perder barriga e definir coxas e bumbum', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2022-07-27'},
  {nome:'Cibele Santos', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'4x por semana', email:'cibele_rs@hotmail.com', telefone:'', piramide:'Bumbum, barriga, coxa, braços', objetivo:'Treinar com mais certeza de que estou fazendo certo, com objetivo e segurança.', restricoes:'Nada grave.; Discopatia na lombar.; Não.', academia:'Yes Fitness', dataAnamnese:'2022-08-08'},
  {nome:'Márcia Burgert', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'marcia.aidealimoveis@gmail.com', telefone:'', piramide:'1 barriga 2 coxa 3 bumbum 4 costas', objetivo:'Emagrecimento', restricoes:'Nenhuma relatada', academia:'Performace', dataAnamnese:'2022-08-10'},
  {nome:'Wilkner Ladwig', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'5x por semana', email:'wilknerladwig34010@gmail.com', telefone:'', piramide:'2', objetivo:'Ganhar muita massa muscular.', restricoes:'Não.; Não.; Não.', academia:'Performance', dataAnamnese:'2022-08-23'},
  {nome:'Kellen Rosynski', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'3x por semana', email:'kellenrosynski86@gmail.com', telefone:'', piramide:'1,2,3,4', objetivo:'Definição muscular,', restricoes:'Nenhuma relatada', academia:'Corpo e forma', dataAnamnese:'2022-08-24'},
  {nome:'Juliana Machado Saucedo', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'juliana.machadosaucedo@gmail.com', telefone:'', piramide:'1- barriga 2- bumbum 3- coxa 4- costas', objetivo:'Emagrecer e definir', restricoes:'Tive uma contratura muscular na lombar', academia:'Yes fitness', dataAnamnese:'2022-08-24'},
  {nome:'Amanda', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'5x por semana', email:'amanda.lilge2012@gmail.com', telefone:'', piramide:'1° barriga 2° bumbum 3° perna 4° costas/braço', objetivo:'Emagrecer e ganhar massa muscular', restricoes:'Nenhuma relatada', academia:'Performance Fitness', dataAnamnese:'2022-09-07'},
  {nome:'Deisi', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'4x por semana', email:'deisirejanesantos@gmail.com', telefone:'', piramide:'1200', objetivo:'Focar e definir o meu corpo de acordo!', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2022-09-29'},
  {nome:'Fabiana Neves da Silveira', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'5x por semana', email:'fabilzrn@gmail.com', telefone:'', piramide:'Bumbum, barriga, coxa, costas', objetivo:'Realizar exercícios corretamente', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2022-10-03'},
  {nome:'Lúcia Scopinski', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'5x por semana', email:'luciascopinski@gmail.com', telefone:'', piramide:'Bumbum, coxa, barriga e tríceps', objetivo:'Definir o corpo', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2022-10-16'},
  {nome:'Rita Fric', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'5x por semana', email:'rita.fric@gmail.com', telefone:'', piramide:'Glúteos, posterior, interna de coxa, abdômen', objetivo:'Redução de gordura e definição', restricoes:'Nenhuma relatada', academia:'Canoas Planet Club e em Lajeado Monte Olimpo', dataAnamnese:'2022-11-09'},
  {nome:'Hellen Teixeira', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'A definir', email:'teixeirahellen22@gmail.com', telefone:'', piramide:'1 bumbum , 2 barriga , 3 coxas , 4 braços .', objetivo:'Conseguir executar os exercícios de forma correta, para melhor resultados tanto na estética quanto mental .', restricoes:'Não .', academia:'Performance', dataAnamnese:'2022-11-15'},
  {nome:'Karina Ribeiro', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'5x por semana', email:'karinaribeirors@gmail.com', telefone:'', piramide:'Coxas, bumbum, braços e costas', objetivo:'Ganhar massa muscular, definir e perder gordura', restricoes:'Torci o tornozelo mas já passou; Diastase abdominal', academia:'As vezes na Performance outras na high fitnes', dataAnamnese:'2022-11-20'},
  {nome:'Ezequiel Rodrigues Trindade', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'ezefuts@gmail.com', telefone:'', piramide:'1-braços, 2-peito, 3-barriga, 4-pernas', objetivo:'Melhora nos resultados dos treinos', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2022-11-20'},
  {nome:'Viviane Pereira', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'5x por semana', email:'vivianepera@hotmail.com', telefone:'', piramide:'2 1 3 4', objetivo:'Emagrecimento e massa magra', restricoes:'Sim estou me curando de uma tandinite no ombro', academia:'Performance', dataAnamnese:'2022-11-23'},
  {nome:'Evelyn Vargas', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'A definir', email:'evelynvargas725@gmail.com', telefone:'', piramide:'1 - Glúteos  2 - Coxas 3 - Barriga  4 - Costas  Exatamente como no exemplo', objetivo:'Meu objetivo principal é recuperar a minha auto estima. Após a minha segunda gestação (são duas cesarianas) tive muito aquele famoso efeito sanfona, isso acabou muito com a minha auto estima. Relaxei por completo. Com a depressão e ansiedade andando juntas acabei piorando. Chegando no auge dos 82kg (atualmente). A diástase também é minha inimiga, não gostaria de me submeter a procedimentos estéticos sem antes tentar de forma natural em construir o corpo dos meus sonhos.', restricoes:'Sim. Quando estava grávida de 3 meses, em 2017, tive muitas crises de dor na coluna. Fazia muita infiltração com o médico que me tratava na época em Guaíba. Como não passava e estava refém da infiltração procurei uma segunda opinião médica. Então fiz a ressonância magnética e constatou degeneração discal.', academia:'Yes Fitness', dataAnamnese:'2022-11-25'},
  {nome:'Diovana Beatriz Gomes Zelake', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'4x por semana', email:'did.zelake@gmail.com', telefone:'', piramide:'Barriga, bumbum, coxa, braço', objetivo:'Definição e uma barriga chapada', restricoes:'Uma dor no ombro quando faço MT esforço', academia:'Em casa', dataAnamnese:'2022-12-05'},
  {nome:'Alana Silva de Oliveira', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'5x por semana', email:'alanavitor.mayapepe@gmail.com', telefone:'', piramide:'Todos', objetivo:'Alcançar minhas metas, evoluir chegar aonde eu nunca cheguei.', restricoes:'Nenhuma relatada', academia:'High fitness', dataAnamnese:'2022-12-05'},
  {nome:'Daiana', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'daianakohls@hotmail.com', telefone:'', piramide:'1 bumbum,2 coxa, 3 barriga 4 costas', objetivo:'Definição de pernas e glúteos', restricoes:'Joelho, acidente de moto doi se subir muita escada', academia:'Em casa', dataAnamnese:'2022-12-05'},
  {nome:'Karina', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'A definir', email:'karina.gds@hotmail.com', telefone:'', piramide:'Barriga  Costas  Bumbum  Coxa', objetivo:'Alcançar meu objetivo de chegar aos 64 kg,e perder um bom pouco da barriga', restricoes:'Luxação na perna a 11 anos', academia:'Em casa/yes fitness', dataAnamnese:'2022-12-06'},
  {nome:'Patrícia Gonzalez', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'patriciagonzalez_rs@hotmail.com', telefone:'', piramide:'1 barriga, 2 coxas, 3 braço, 4 bumbum, 5 costas', objetivo:'Emagrecer, perder barriga, definir o corpo', restricoes:'Nenhuma relatada', academia:'Engenharia do corpo Passo dareia', dataAnamnese:'2022-12-09'},
  {nome:'camilly', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'camillyvescovi40@gmail.com', telefone:'', piramide:'1-barriga 2-bumbum 3-coxa 4-costas', objetivo:'ter resultados melhor', restricoes:'não tive; não tenho', academia:'gauleses', dataAnamnese:'2023-01-09'},
  {nome:'Viviane goulart', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'vivianegoulart97@gmail.com', telefone:'', piramide:'1,2,4,3.', objetivo:'Aprendizado, crescimento de massa muscular.', restricoes:'Megapófise transversa bilateral em L5.', academia:'Academia corpo e forma.', dataAnamnese:'2023-01-09'},
  {nome:'Samanta', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'3x por semana', email:'samantagoulart19@gmail.com', telefone:'', piramide:'Barriga, bumbum, coxa, costas', objetivo:'Ter resultados e aprender mais.', restricoes:'Nenhuma relatada', academia:'Academia Corpo e forma', dataAnamnese:'2023-01-09'},
  {nome:'Bruna Melissa Morais Ferreira', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'6x por semana', email:'bruna.melissa4@gmail.com', telefone:'', piramide:'1- glúteos, 2- barriga, 3 costas, 4- coxas', objetivo:'Experimentar a consultoria com toda a minha dedicação e ver os resultados q consigo.', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2023-01-11'},
  {nome:'Brendha', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'A definir', email:'brendharodrigues3@gmail.com', telefone:'', piramide:'1-Barriga, 2-Bumbum, 3-braços e 4- coxas', objetivo:'Emagrecimento', restricoes:'Sim, muai thay , joelho esquerdo incomoda um pouco as vezes. (Raramente)', academia:'Performance fitness', dataAnamnese:'2023-01-11'},
  {nome:'Bruno Freitas Gadea', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'bfg_bruno@hotmail.com', telefone:'', piramide:'1-Abdomem, 2-peito, 3-braços, 4-costas', objetivo:'Buscar um corpo mais bonito, utilizando de forma correta e eficiente os exercícios, chegando a um resultado com saúde.', restricoes:'Dedo em gatilho no dedo médio da mão esquerda (mas não atrapalha nos exercícios). Pretendo operar no 1º semestre de 2023.; Dedo em gatilho na mão direita (nov 2022).', academia:'Performance Fitness (estou me inscrevendo)', dataAnamnese:'2023-01-11'},
  {nome:'Marília Elusa dos Santos Brites', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'4x por semana', email:'mariliasantosbrites@gmail.com', telefone:'', piramide:'Barriga, braço, coxa, bumbum', objetivo:'Perder gordura, ganhar massa muscular', restricoes:'Escápulas dos joelhos deslocam', academia:'Engenharia do corpo', dataAnamnese:'2023-01-11'},
  {nome:'Douglas soares', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'5x por semana', email:'doug_mega@hotmail.com', telefone:'', piramide:'Costas, peitoral, pernas, barriga', objetivo:'Hipertrofia', restricoes:'Nenhuma relatada', academia:'Usina do corpo', dataAnamnese:'2023-01-12'},
  {nome:'Luciane fonseca', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'lucianefonseca1980@gmail.com', telefone:'', piramide:'1-2-4', objetivo:'Definição', restricoes:'Nenhuma relatada', academia:'Nation', dataAnamnese:'2023-01-15'},
  {nome:'DAIA Pires trentim', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'5x por semana', email:'daianepires201413@gmail.com', telefone:'', piramide:'Coxa', objetivo:'Bombum coxa barriga', restricoes:'51980102684', academia:'Performance', dataAnamnese:'2023-01-15'},
  {nome:'Karina Saucedo', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'ksaucedo_rs@yahoo.com', telefone:'', piramide:'1 - barriga  2 - coxa 3 - bumbum 4 - costas', objetivo:'Emagrecimento e hipertrofia', restricoes:'Tive uma cesárea há 4 meses, mas até agora não me limitou.', academia:'MW Porto Alegre', dataAnamnese:'2023-01-17'},
  {nome:'Camila Goulart Camboim', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Intermediário', freq:'3x por semana', email:'camila.goulart_1@hotmail.com', telefone:'', piramide:'1° abdômen, 2° braços/costas,  3° bunda, 4° coxas....tipo tudo!', objetivo:'Fortalecer musculatura geral, principalmente abdominal, tirar excesso de gordura nas costas e braços, culotes, definir o corpo.', restricoes:'Tenho Psoríase e uma dor crônica em todo lado direito, discopatia degenerativa, esclerose óssea subcondral, coxartrose, artrite e artrose psorisiática.; Socroileíte bilateral, artropatia degenerativa coxofemoral, tendinopatia do glúteo mínimo e do médio. Estou encaminhando laudos.', academia:'Academia Corpo e Alma-Shopping Total', dataAnamnese:'2023-01-17'},
  {nome:'Gabriel Brites', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'6x por semana', email:'britesgabriel253@gmail.com', telefone:'', piramide:'bíceps,peito,costa,pernas', objetivo:'definição', restricoes:'Nenhuma relatada', academia:'power estancia velha', dataAnamnese:'2023-01-23'},
  {nome:'Evelin', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'evelin.oprach@hotmail.com', telefone:'', piramide:'coxas … bunda … costas … abdômen', objetivo:'Ganho de resistência e de musculatura', restricoes:'Nenhuma relatada', academia:'Eldorado - guaiba ideia', dataAnamnese:'2023-01-24'},
  {nome:'MARIELE', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'mariele.saez@gmail.com', telefone:'', piramide:'3 2 1 4', objetivo:'Perder gordura, fortalecer músculos e treinar para o taf', restricoes:'Nenhuma relatada', academia:'.', dataAnamnese:'2023-02-10'},
  {nome:'Maria Luiza Souza Pereira', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'5x por semana', email:'luiza_luh09@hotmail.com', telefone:'77999667009', piramide:'1- bumbum 2-coxa 3- barriga 4-superior no geral', objetivo:'Desenvolver a minha musculatura e evoluir nos treinos', restricoes:'Nenhuma relatada', academia:'Extreme sports', dataAnamnese:'2023-03-05'},
  {nome:'Michele Sirlem Fagundes Cirne', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'5x por semana', email:'michelefagundescirne@gmail.com', telefone:'(51)996020374', piramide:'1- barriga, 2- bumbum, 3- coxa e 4 -braço', objetivo:'Perda de peso e definição', restricoes:'Nenhuma relatada', academia:'Em casa', dataAnamnese:'2023-03-07'},
  {nome:'Carine da Silva trapp', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Intermediário', freq:'3x por semana', email:'carinetrapp55@gmail.com', telefone:'51989408029', piramide:'1 - bumbum, 2- coxa, 3 -barriga , 4- costas', objetivo:'Olhar no espelho e me sentir bem com o que vejo, um corpo bonito', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2023-03-07'},
  {nome:'Gabriela Jacobsen', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'A definir', email:'bgabrielajacobsen@gmail.com', telefone:'51996063772', piramide:'Barriga, costas, braços e bumbum', objetivo:'Trabalharmos juntos para obter um resultado satisfatório sobre meu corpo e criar bons hábitos com a musculação', restricoes:'Realizei cesárea', academia:'Performance fitness', dataAnamnese:'2023-03-15'},
  {nome:'Sílvia', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'4x por semana', email:'silviastltransportes@outlook.com.br', telefone:'51 995996941', piramide:'1ºbumbum,2º coxas, 3º barriga, 4º costas', objetivo:'Ganhar massa magra, perder gordura e definir', restricoes:'Nenhuma relatada', academia:'Perfomance fitness- Eldorado do Sul', dataAnamnese:'2023-03-17'},
  {nome:'Aline', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'5x por semana', email:'alinedutracosme2000@gmail.com', telefone:'51985312118', piramide:'1, 2, 3 e 4', objetivo:'Ganho de massa', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2023-03-27'},
  {nome:'Betina', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'4x por semana', email:'betina.vom@gmail.com', telefone:'51995592955', piramide:'Bumbum, peenas, barriga e bracos', objetivo:'Emagrecer e criar massa', restricoes:'Nenhuma relatada', academia:'Usina do corpo', dataAnamnese:'2023-04-04'},
  {nome:'Bruna Melo Soares', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'brunamelosoares@gmail.com', telefone:'51992293575', piramide:'Barriga,bumbum,coxas e costas', objetivo:'Emagrecer', restricoes:'Nenhuma relatada', academia:'Perfomance', dataAnamnese:'2023-04-10'},
  {nome:'erica da silva abreu', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'6x por semana', email:'ericadasilvaabreu117@gmail.com', telefone:'51995098335', piramide:'1- coxa 2- barriga  3- glúteo  4- costas', objetivo:'me dedicar e alcançar meu objetivos', restricoes:'Nenhuma relatada', academia:'performance', dataAnamnese:'2023-05-13'},
  {nome:'Carolina da Silva abreu', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Intermediário', freq:'5x por semana', email:'abreuc916@gmail.com', telefone:'‪+55 51 98062‑0074‬', piramide:'1', objetivo:'Criar corpo', restricoes:'Nenhum', academia:'Academiaperformacef', dataAnamnese:'2023-05-13'},
  {nome:'Letícia Graff', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'A definir', email:'leticiagraff123@gmail.com', telefone:'51980593827', piramide:'Emagrecimento 1 3 4', objetivo:'Emagrecimento e definição', restricoes:'Nenhuma relatada', academia:'Yes', dataAnamnese:'2023-05-14'},
  {nome:'Maristela Brandão', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Intermediário', freq:'4x por semana', email:'maristelabrandao99@gmail.com', telefone:'51989224998', piramide:'Braço  Coxas Bumbum  Barriga', objetivo:'Emagrecimento e definição', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2023-05-14'},
  {nome:'Rafaela Mesquita Gouvea', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'rafa.gouvea@hotmail.com', telefone:'51985642726', piramide:'1°bumbum 2°coxa 3°barriga 4° definir braço', objetivo:'Auxílio para emagrecer e definição na musculatura', restricoes:'Tive, bursite no quadril e rompi ligamentos do tornozelo direito', academia:'Performance', dataAnamnese:'2023-05-16'},
  {nome:'Juliano de Souza Machado', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'juliano38-machado@outlook.com', telefone:'51998741214', piramide:'1- bíceps 2- peito 3- costas 4- panturrilha', objetivo:'Ter bons resultados', restricoes:'Nenhuma relatada', academia:'Sublime - Alvorada/RS', dataAnamnese:'2023-05-28'},
  {nome:'KARINE DE FÁTIMA TRINDADE BARBOSA', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'kaka.fisioterapeuta@gmail.com', telefone:'51992369212', piramide:'1- BARRIGA 2-COSTAS- 3-BRAÇOS - 4-PERNAS 5- BUMBUM', objetivo:'REDUZIR PESO, DEFINIR MUSCULATURA E GANHODE FORÇA MUSCULAR', restricoes:'CONDROMALÁCIA PATELAR E', academia:'PERFORMANCE', dataAnamnese:'2023-06-01'},
  {nome:'Rovena Frenzel', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'5x por semana', email:'ro.frenzel@gmail.com', telefone:'51981457664', piramide:'Exatamente esta ordem - bumbum, coxa, barriga, costas', objetivo:'Me ajudar a ter uma sequência de exercícios eficazes, levando a um corpo mais proporcional e harmonioso, ao invés de eu ficar perdida fazendo exercícios que de repente, não atinjam meus objetivos.', restricoes:'Tenho duas hérnias de disco na lombar, L4-L5 e L5- S1. Sei exatamente que preciso de exercícios pra fortalecimento do core e no geral pra não ter crises/dores e tbem procuro cuidar muito a postura/execução e tipo de exercício.; Só as hérnias que falei acima.', academia:'Performance Fitness', dataAnamnese:'2023-06-06'},
  {nome:'Angélica Fric', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'5x por semana', email:'nutricionistafric@gmail.com', telefone:'51991416225', piramide:'1- glúteos 2- coxas (lateral principalmente) 3 - abdômen 4 - superiores', objetivo:'Hipertrofia,  para superiores não tem problema treinar “pesado”, tenho maior dificuldade em inferiores devido as dores (exames que te mandei). Então se eu conseguir ficar com 60kg mas com maior volume muscular, eu fico bem feliz.', restricoes:'Sim, tá nos exames que enviei.; Já foi enviado.', academia:'Treino na Planet Club em Canoas, mas pretendo mudar para a Engenharia do Corpo mês que vem.', dataAnamnese:'2023-06-10'},
  {nome:'Yasmin Flores de Oliveira', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'5x por semana', email:'yasminflores550@gmail.com', telefone:'51 984562913', piramide:'1° bumbum 2° coxa 3° barriga  4° costas', objetivo:'Ganhar massa muscular e ter definição.', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2023-06-26'},
  {nome:'Kenya Balz', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'3x por semana', email:'kenyabalz.16@gmail.com', telefone:'51993634809', piramide:'1 - barriga, 2 - costas, 3 - bumbum, 4 - coxas', objetivo:'Perder peso com o menor grau de flacidez possível e definição muscular', restricoes:'Nenhuma relatada', academia:'Academia Mecca - centro - Gravataí', dataAnamnese:'2023-06-27'},
  {nome:'Luciane Schmitt', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'6x por semana', email:'lu.mendes.marques@gmail.com', telefone:'(51)991714828', piramide:'Exatamente a ordem acima', objetivo:'Resultado', restricoes:'Ciático perna direita e braço esquerdo (tendinite); Sim', academia:'Performance', dataAnamnese:'2023-06-28'},
  {nome:'Priscila Bissigo', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'4x por semana', email:'priscila.bissigo@hotmail.com', telefone:'51982889430', piramide:'Abdômen - glúteos - coxas - braços - costas', objetivo:'Potencializar os resultados', restricoes:'Nenhuma relatada', academia:'Yes', dataAnamnese:'2023-07-02'},
  {nome:'Mariana Santana', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'A definir', email:'periciasrt@yahoo.com.br', telefone:'51999053099', piramide:'Bumbum e barriga', objetivo:'Definir', restricoes:'Nenhuma relatada', academia:'Smart', dataAnamnese:'2023-07-03'},
  {nome:'Mariana conzatti', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'4x por semana', email:'marianaconzatti97@gmail.com', telefone:'51981858087', piramide:'1- coxa 2- glúteo 3- abdômen 4- braço', objetivo:'Ganho de massa magra e definição', restricoes:'Lombar, joelhos; Leve desgaste de cartilagem dos joelhos', academia:'Smart fit', dataAnamnese:'2023-07-07'},
  {nome:'Maiara Santos', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'maiarasantosvez@gmail.com', telefone:'51992851011', piramide:'1 barriga  2 bumbum  3 coxas  4 costas', objetivo:'Perder peso e ganhar definição', restricoes:'Epicondilite no cotovelo direito', academia:'Moinhos fitness', dataAnamnese:'2023-07-07'},
  {nome:'Eduardo barbosa oliveira', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'Eduardobarbosaoliveira10@gmail.com', telefone:'51991941368', piramide:'1_abdomem 2-peito3- costas4-pernas', objetivo:'Perda de peso e ganho de massa muscular', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2023-07-10'},
  {nome:'Carla hendler evaldt', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'5x por semana', email:'carlahendler@yahoo.com', telefone:'51 982161600', piramide:'1 bumbum 2 coxa 3 barriga e cintura 4 costas', objetivo:'Definir corpo e ganhar massa muscular', restricoes:'Nenhuma relatada', academia:'Yes fitnes em Eldorado do Sul', dataAnamnese:'2023-07-17'},
  {nome:'Victoria shakur', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'6x por semana', email:'vicshakur78@icloud.com', telefone:'51985446487', piramide:'1 glúteo  2 coxa 3 costas  4 abdômen', objetivo:'Hipertrofia', restricoes:'Naoo', academia:'Yes fitness', dataAnamnese:'2023-07-20'},
  {nome:'Michel Borges Evaldt', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'carlaehendler@gmail.com', telefone:'51999597597', piramide:'1 peitoral, 2braco, 3 abdômen, 4 perna', objetivo:'Definir o corpo', restricoes:'Nenhuma relatada', academia:'Yes fitnes', dataAnamnese:'2023-07-26'},
  {nome:'Juliana Oliveira de Sousa', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Intermediário', freq:'6x por semana', email:'sensiju@gmail.com', telefone:'21990442382', piramide:'1- Coxa, 2- barriga, 3- bumbum, 4-costas', objetivo:'Emagrecer e ter um corpo definido', restricoes:'Nenhuma relatada', academia:'Pratick academia (em sobral)', dataAnamnese:'2023-07-26'},
  {nome:'Aline De Santana Diaz Machado', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'5x por semana', email:'alinesdmachado@gmail.com', telefone:'51 986517741', piramide:'Glúteo, quadríceps e posterior (porém o foco é tudo, até superiores haha)', objetivo:'Hipertrofia', restricoes:'Nenhuma', academia:'Titanium', dataAnamnese:'2023-07-27'},
  {nome:'Caroline Castro da Silveira', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'5x por semana', email:'ccastrosilveira@gmail.com', telefone:'51991672778', piramide:'1 - Glúteo 2 - Coxas 3 - Costas 4 - Braços', objetivo:'Melhora nos resultados', restricoes:'Sim, fratura da clavícula direita', academia:'Performance', dataAnamnese:'2023-07-28'},
  {nome:'Daniele Arl Vertuoso Salvador', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'4x por semana', email:'danivertuoso@gmail.com', telefone:'47992106298', piramide:'1 - bumbum, 2 - quadriceps, 3 - abdômen, 4 - costas', objetivo:'ganhar força muscular', restricoes:'Nenhuma relatada', academia:'movefit', dataAnamnese:'2023-07-28'},
  {nome:'Katiusca dos Santos Oliveira', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'oliveirakatisantos.ks@gmail.com', telefone:'51 997837527', piramide:'3412', objetivo:'Perder barriga e medidas', restricoes:'Nenhuma relatada', academia:'Performace/ B12', dataAnamnese:'2023-08-01'},
  {nome:'Kamilly Oliveira Tiburski', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'millytiburski08@icloud.com', telefone:'51997117981', piramide:'1 bumbum', objetivo:'coxa definida,bumbum redondo e grande,afinar a cintura e me manter sem barriga', restricoes:'Nenhuma relatada', academia:'casa ou performance', dataAnamnese:'2023-08-01'},
  {nome:'Daiane  Pires trentim', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'4x por semana', email:'daianepires@gamail.com.br', telefone:'980102684', piramide:'1', objetivo:'Perder peso ganhar massa', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2023-08-02'},
  {nome:'Gabriela de Souza Miguel', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'4x por semana', email:'gabrielasouzamiguel@gmail.com', telefone:'(35) 99926-3773', piramide:'1- bumbum 2- barriga 3- coxa 4- costas', objetivo:'Perder peso, perder barriga (pochete), aumentar glúteos e coxa, afinar a cintura e ficar gostosa🤷', restricoes:'Sim, no braço esquerdo; Fiz abaixo do osso do cóccix', academia:'Workout', dataAnamnese:'2023-08-02'},
  {nome:'Samantha dos Santos de Oliveira', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'samantha.santos2@yahoo.com.br', telefone:'51981324886', piramide:'1°barriga,2-bumbum,3-coxa,4-costas', objetivo:'Recuperar minha autoestima e construir uma rotina de exercícios.', restricoes:'Nenhuma relatada', academia:'TopFitness Academia', dataAnamnese:'2023-08-02'},
  {nome:'Amanda Lacerda', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Intermediário', freq:'5x por semana', email:'lacerdaamanda103@gmail.com', telefone:'51992772709', piramide:'1-bumbum 2-coxa toda 3- costas 4- barriga', objetivo:'Meu objetivo é eliminar as gorduras e criar músculos', restricoes:'Nenhuma relatada', academia:'Blacksullfitness', dataAnamnese:'2023-08-02'},
  {nome:'Lisa Rodrigues Naiff', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'5x por semana', email:'lisanaiff69@gmail.com', telefone:'(51)992062805', piramide:'3', objetivo:'Emagrecer', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2023-08-03'},
  {nome:'Raquel Frenzel', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'3x por semana', email:'raquelfrenzel@gmail.com', telefone:'(51) 999446617', piramide:'Tudo, inclusive MSS', objetivo:'Reforço muscular, emagrecimento', restricoes:'Canelite', academia:'Na performance ou na yes fitnes (aceito sugestões)', dataAnamnese:'2023-08-05'},
  {nome:'Leonardo Colombo', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'leolimao210582@gmail.com', telefone:'48 998678948', piramide:'Peito , abdômen, bunda , perna', objetivo:'Peder peso e definição', restricoes:'Sim.  Ombro; Punho', academia:'Engenharia do Corpo', dataAnamnese:'2023-08-06'},
  {nome:'Millene salvado', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'6x por semana', email:'millenesalvado83@gmail.com', telefone:'51986490186', piramide:'1 bumbum 2 coxas  3 barriga  4 costas', objetivo:'Crescimento', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2023-08-09'},
  {nome:'Analu Carvalho', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Intermediário', freq:'3x por semana', email:'ana@caberaf.com.br', telefone:'51998308830', piramide:'1-abdomem 2- cintura 3-pernas e interno das coxas  4- costas', objetivo:'Alcançar com Êxito meu Objetivo', restricoes:'Quebrei  o tornozelo direito, tenho 9 pinos e uma placa, dói as vezes, e tbm nao consigo abaixar muitooo , por exemplo de cócoras nao consigo ficar; tenho Ernia de disco na coluna cervical X5 e X6, escapei de cirurgia que estava agendada para 12/04, mas como sai da crise pedi ao medico que suspendesse; tornozelo direito', academia:'Performance', dataAnamnese:'2023-08-14'},
  {nome:'Leon Kayro', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'A definir', email:'leonkayro123@gmail.com', telefone:'51982328548', piramide:'Tudo', objetivo:'Me curar', restricoes:'Sim', academia:'Performance', dataAnamnese:'2023-08-16'},
  {nome:'Thales Fontoura Soares', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'6x por semana', email:'thales_fontoura@hotmail.com', telefone:'51997603480', piramide:'Ombros, perna, costas, bicep e tríceps, peito', objetivo:'Ficar mais gato', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2023-08-21'},
  {nome:'Nathali Albuquerque', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'nathalbuq@gmail.com', telefone:'51989251100', piramide:'1. Barriga, 2. Coxa, 3. Braço, 4. Bumbum', objetivo:'Emagrecer pelo menos 10kg, perder barriga e definir. Tive uma gestação em 2020 que foi até 42 semanas e deixou a barriga bem flácida. Exercícios pra postura também são bem vindos, trabalho 8h direto sentada. Aumentar resistência física.', restricoes:'Rompi o ligamento do tornozelo em dezembro e as vezes ainda sinto um leve desconforto', academia:'High ou Performance', dataAnamnese:'2023-08-21'},
  {nome:'Naiara Machado', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'3x por semana', email:'naiara_machado@hotmail.com', telefone:'51980558557', piramide:'3, 1, 2, 4', objetivo:'Emagrecimento e hipertrofia', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2023-08-22'},
  {nome:'Vivian Oliveira da Silva', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'vivianoliveiradasilva23@gmail.com', telefone:'51989766806', piramide:'1 barriga 2 bumbum 3 coxas 4 costas', objetivo:'Perder peso e ganhar glúteos', restricoes:'Nenhuma relatada', academia:'Cia do corpo', dataAnamnese:'2023-09-01'},
  {nome:'Camila da Silva Estevão', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'4x por semana', email:'camila_estevao15@hotmail.com', telefone:'51 991978368', piramide:'Quadríceps; glúteo; panturrilha; barriga', objetivo:'Hipertrofia', restricoes:'Nenhuma relatada', academia:'Moinhos fitness', dataAnamnese:'2023-09-02'},
  {nome:'Caroline', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'3x por semana', email:'carollalgayer@icloud.com', telefone:'51997975459', piramide:'1 pernas , 2 bumbum, 3 barriga, 4 costas (coluna)', objetivo:'Ganhar definição de pernas e glúteo', restricoes:'Hiperlordose na cervical e lombar e discopatia degenerativa da lombar e da cervical', academia:'Bodytech', dataAnamnese:'2023-09-04'},
  {nome:'Joao carlos l de souza filho', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'4x por semana', email:'joaocarloslemos91@gmail.com', telefone:'51989502000', piramide:'1 barriga 2 peito 3 costas 4 braços', objetivo:'Obter auxílio para. Conseguir resultados mais rapidamente', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2023-09-05'},
  {nome:'Atricia cardoso', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'3x por semana', email:'atricia.cardoso@gmail.com', telefone:'51995655212', piramide:'1 2 3 4', objetivo:'Definição', restricoes:'Coluna lombar', academia:'Eldo e poa', dataAnamnese:'2023-09-09'},
  {nome:'Mariana Medeiros Castanha', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'castanhamedeiros1@gmail.com', telefone:'(51)98413-3054', piramide:'1, 2, 3 e 4', objetivo:'Fortalecimento muscular e definição', restricoes:'Já tive um tumor no quadril e devido a isso, em alguns aparelhos, sinto um pouco de sensibilidade na região', academia:'Performance', dataAnamnese:'2023-09-18'},
  {nome:'Daiandra Gonçalves', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'daiandra.dasilva@gmail.com', telefone:'51 984809431', piramide:'1 barriga 2 bumbum 3 coxas 4 costas', objetivo:'Voltar ao meu corpo de antes das gestações', restricoes:'Nenhuma relatada', academia:'Em casa', dataAnamnese:'2023-09-20'},
  {nome:'Priscila Justo Becker', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'5x por semana', email:'priscilajbecker20@gmail.com', telefone:'(51) 995987375', piramide:'1 - Gluteos 2 - Coxa 3 - Braços (biceps, triceps, ombro - normalmente acumulo gordura nessa região) 4 - Costas', objetivo:'conseguir um treino que realmente me traga resultados e seja focado no meu corpo', restricoes:'Nenhuma relatada', academia:'Engenharia do corpo - Petrópolis', dataAnamnese:'2023-09-21'},
  {nome:'Pablo Leonardo', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'pablodudu2010@gmail.com', telefone:'51999266207', piramide:'1 - Shape 2 - bíceps 3 - Costas 4 - Pernas', objetivo:'Consistência e apoio profissional', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2023-10-08'},
  {nome:'Luciana Machado', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'5x por semana', email:'lumachadorecova02@gmail.com', telefone:'51995502031', piramide:'1 Barriga,  2 bumbum,  3 coxa,  4 costas', objetivo:'Primeiro lugar pela saúde, segundo, definir o corpo', restricoes:'Nenhuma relatada', academia:'Sesc', dataAnamnese:'2023-10-10'},
  {nome:'Adriana Do Couto Schnadelbach', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'6x por semana', email:'schnadelbachadriana@gmail.com', telefone:'51995027512', piramide:'1- quadricips, 2- glúteo, posteiror e abdômen', objetivo:'Ter um treino alinhado que eu evoluía e tenha ótimos resultados.', restricoes:'Vascular', academia:'Performance', dataAnamnese:'2023-10-23'},
  {nome:'Bárbara Darski', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'darski.barbara@gmail.com', telefone:'51 994537683', piramide:'1 - barriga, 2 - bumbum, 3 - pernas, 4 - braços', objetivo:'Perder peso, ter qualidade de vida e definir meu corpo', restricoes:'Nunca tive, mas possuo condropatia patelar nos dois joelhos, fator genético; Condropatia patelar, grau 3 no joelho direito e grau 2 no joelho esquerdo; Nunca', academia:'Sesc Centro Histórico', dataAnamnese:'2023-10-23'},
  {nome:'Alisson Bruno Oliveira da Cunha', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'alisson.obruno@gmail.com', telefone:'51996387029', piramide:'1-Peito, 2-Biceps, 3- coxa, 4- costas', objetivo:'Ganho de massa muscular e definição.', restricoes:'Nenhuma relatada', academia:'Engenharia do corpo', dataAnamnese:'2023-10-23'},
  {nome:'Mari Dantas', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'5x por semana', email:'marisete.osorio@hotmail.com', telefone:'51985099341', piramide:'1Barriga 2costas 3Bumbum 4 coxa', objetivo:'Emagrecer e ganhar massa muscular', restricoes:'Nunca tive; Ñ; Ñ', academia:'Barra Thay', dataAnamnese:'2023-11-16'},
  {nome:'Monica Colombo', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'A definir', email:'monicacolombo2013@gmail.com', telefone:'+5551998080439', piramide:'3 2 1 4', objetivo:'Diminuir medidas e definir perna coxa barriga e bumbum', restricoes:'Pé torção', academia:'Bem estar Cachoeirinha', dataAnamnese:'2023-11-20'},
  {nome:'Natália Rodrigues', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'5x por semana', email:'nathyrodriguess_@hotmail.com', telefone:'51996405068', piramide:'1• bumbum, 2 • coxa, 3 • barriga e 4 • bíceps', objetivo:'Definir bumbum, coxas (tenho muita dificuldade em definir quadríceps), secar barriga', restricoes:'Nenhuma relatada', academia:'Academia Performance', dataAnamnese:'2023-11-20'},
  {nome:'Rubia Bagesteiro', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'bagesteiro.r@gmail.com', telefone:'51995073871', piramide:'1- bumbum, 2-coxa, 3- braços, 4- costas', objetivo:'Diminuir o percentual de gordura e hipertrofia', restricoes:'Nenhuma relatada', academia:'Em primeiro momento em casa, mas penso em futuramente treinar em alguma academia de Eldorado', dataAnamnese:'2023-11-20'},
  {nome:'Thaila Gomes', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Intermediário', freq:'6x por semana', email:'thailaandry@gmail.com', telefone:'51994224479', piramide:'3,1,2,4', objetivo:'Saúde e alto estima', restricoes:'Nenhuma relatada', academia:'Arena Humaitá', dataAnamnese:'2023-11-20'},
  {nome:'Luciana Teixeira', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'A definir', email:'lucianaitaquy@gmail.com', telefone:'51 99180.9978', piramide:'3', objetivo:'Emagrecimento', restricoes:'Nenhuma relatada', academia:'Academia do meu predio por enquanto. Acredito que tenha todos os aparelhos abaixo. Tem bastante pessoas que estão malhando na academia, fazendo musculação.', dataAnamnese:'2023-11-20'},
  {nome:'Flávia da Silva', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'A definir', email:'bemvindocliente8101@gmail.com', telefone:'51984681781', piramide:'1- barriga 2- bumbum 3-coxas 4- costas', objetivo:'Melhorar a saúde mental e física ( mto sedentária)', restricoes:'Nenhuma relatada', academia:'Ainda n sei , mas assim que der a YES', dataAnamnese:'2023-11-24'},
  {nome:'Jessica Cabral', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'5x por semana', email:'jessiicabral@live.com', telefone:'51989550503', piramide:'1 barriga 2 costas 3 bumbum 4 coxa', objetivo:'Hipertrofia quero definição no corpo sinto que os inferiores até mudou mas superiores estagnou', restricoes:'Nenhuma relatada', academia:'Yes', dataAnamnese:'2023-11-24'},
  {nome:'Claudete Dantas Rolim', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'4x por semana', email:'clau.rolim8@gmail.com', telefone:'51 99448-9396', piramide:'Os 4', objetivo:'Tentar mudar um pouco o meu corpo', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2023-11-28'},
  {nome:'Helen Caroline Brites dos Santos', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'4x por semana', email:'helenbrites342@gmail.com', telefone:'54991550436', piramide:'1- coxa 2-barriga 3- costas 4- bumbum', objetivo:'Atingir o peso ideal e ter o corpo definido', restricoes:'Leve desvio na coluna vertebral.', academia:'Engenharia do corpo', dataAnamnese:'2023-12-17'},
  {nome:'Sonia marzona', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'2x por semana', email:'soniamariamarzona@gmail.com', telefone:'51994527474', piramide:'1) bumbum e costas 2) barriga 3) braço 4) coxa', objetivo:'Definir musculos e emagrecer', restricoes:'Nenhuma relatada', academia:'Performance fitness', dataAnamnese:'2024-01-04'},
  {nome:'Monique Gonzalez Pereira', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'moniquepereira645@gmail.com', telefone:'51993721614', piramide:'1 bumbum', objetivo:'Me ajudar a crescer os músculos', restricoes:'Nunca tive', academia:'Performance', dataAnamnese:'2024-01-05'},
  {nome:'Jéssica Stéphanie', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'2x por semana', email:'jessica.nasario@gmail.com', telefone:'51-985076707', piramide:'Coxa, bumbum, barriga, braço', objetivo:'Definição muscular', restricoes:'Bursite no ombro direito; cervical retilínea; dores no joelho direito; Sim, bursite e cervical', academia:'BodyGym (Guaíba)', dataAnamnese:'2024-01-09'},
  {nome:'Franciele', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'4x por semana', email:'franciele.morialdo@hotmail.com', telefone:'51995517353', piramide:'Bumbum, coxa, cintura, barriga (gosto de treinar costas também)', objetivo:'Ganhar massa muscular', restricoes:'Nenhuma relatada', academia:'Smart Fit menino Deus', dataAnamnese:'2024-01-10'},
  {nome:'Bruna Pinós Rodzinski', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'rodzinskibruna@gmail.com', telefone:'51997209023', piramide:'1 coxa 2 bumbum 3 barriga 4 braço', objetivo:'hipertrofia e emagrecimento', restricoes:'Nenhuma relatada', academia:'performance', dataAnamnese:'2024-01-19'},
  {nome:'Gisele', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'3x por semana', email:'gymanke1986@gmail.com', telefone:'51994031298', piramide:'Bumbum , coxa , barriga e braços', objetivo:'Um corpo definido', restricoes:'Tive na panturrilha esquerda ( trombose ) a seis meses', academia:'Yes fitness', dataAnamnese:'2024-01-22'},
  {nome:'Luzeni Alves de oliveira Teixeira', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'5x por semana', email:'luzenir329@gmail.com', telefone:'65981325837', piramide:'1.3.4', objetivo:'Fazer os exercícios certo', restricoes:'Nenhuma relatada', academia:'Olímpica', dataAnamnese:'2024-02-08'},
  {nome:'Danielli', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'5x por semana', email:'dfuchs347@gmail.com', telefone:'(51) 989241007', piramide:'1342', objetivo:'Perder peso e ganhar definição muscular', restricoes:'Sentia muita dor no joelho antes de começar a musculação. Já tive problemas na lombar e recentemente senti desconforto na mesma. Quando joguei vôlei ano passado tive uma leve lesão no ombro direito. E tmbm ando sentindo desconforto na escapula direita 🤡; Nada diagnosticado', academia:'Perfomance', dataAnamnese:'2024-02-12'},
  {nome:'Cristiane Soares', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'6x por semana', email:'cristiane_oliveiraosares@hotmail.com', telefone:'51998313443', piramide:'1- bumbum 2- braço 3- barriga - costas', objetivo:'Fortalecimento', restricoes:'Nenhuma relatada', academia:'Engenharia', dataAnamnese:'2024-02-16'},
  {nome:'Marilda', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'A definir', email:'Marilda1972@hotmail.com', telefone:'7865862453', piramide:'1coxa  2 barriga3 bumbum', objetivo:'Treinar com qualidade', restricoes:'Nenhuma relatada', academia:'You Fit', dataAnamnese:'2024-02-21'},
  {nome:'Michele Abreu', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'5x por semana', email:'michele.abreu@auxiliadorapredial.com.br', telefone:'51999571979', piramide:'1- barriga, 2- bumbum, 3- coxas 4- costas', objetivo:'Máximo de ganho muscular possível', restricoes:'Nenhuma relatada', academia:'Performance Fitness', dataAnamnese:'2024-03-07'},
  {nome:'Marilene Lopes Mendonça', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Intermediário', freq:'6x por semana', email:'ml4103766@gmail.com', telefone:'1978 7287503', piramide:'Costa', objetivo:'Perca de peso', restricoes:'Nenhuma relatada', academia:'Não sei o nome', dataAnamnese:'2024-03-14'},
  {nome:'Maria Clara Gomes Brito', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'6x por semana', email:'maclaragomes.b@gmail.com', telefone:'51 996155298', piramide:'1- Barriga 1- Coxa 2- Superiores em geral 4- Bumbum', objetivo:'Eu gostaria de perder gordura e ganhar músculo.', restricoes:'Tenho uma fissura no joelho leve, causada provavelmente por condromalácia patelar.; Vou enviar à ressonância do joelho.; Não.', academia:'Performance de Eldorado.', dataAnamnese:'2024-03-15'},
  {nome:'Eduarda da Silva coelho', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'6x por semana', email:'eduardacoeelho772@gmail.com', telefone:'(51)991983630', piramide:'2143', objetivo:'Hipertrofia', restricoes:'Nenhuma relatada', academia:'-', dataAnamnese:'2024-03-22'},
  {nome:'Elis Agostini', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'3x por semana', email:'elis_agostini@hotmail.com', telefone:'51996901050', piramide:'1 - gluteos 2 - perna quadríceps, posterior  3 - abdômen', objetivo:'Definição e ganho de massa muscular', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2024-03-26'},
  {nome:'Ana carolina luigi', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'5x por semana', email:'anacarolinacruzbessa@gmail.com', telefone:'321-9469794', piramide:'Bumbum, coxa, barriga, costas', objetivo:'Definir( acima não sei bem os nomes das máquinas ) então não sei o que tem )', restricoes:'Nenhuma relatada', academia:'Planet fitness', dataAnamnese:'2024-03-29'},
  {nome:'Suehen', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'nattalleoliveira@hotmail.com', telefone:'5616744100', piramide:'1 barriga ,2 braços 3 pernas 4 costas', objetivo:'Aprender a me exercitar', restricoes:'No joelho', academia:'Ufit', dataAnamnese:'2024-04-04'},
  {nome:'Karin Paliosa', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'A definir', email:'karin.radiologia@gmail.com', telefone:'51 993616934', piramide:'Glúteo, perna, costas, barriga', objetivo:'Aumentar massa magra, definição, aumentar glúteo', restricoes:'Nenhuma relatada', academia:'Fenix', dataAnamnese:'2024-04-05'},
  {nome:'Natália Andrade', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'ailatan_edardna@hotmail.com', telefone:'051984656980', piramide:'Barriga/Bumbum/coxa/costas', objetivo:'Definição, emagrecimento, bem estar', restricoes:'Nenhuma relatada', academia:'Idéia Fitness Academia (Santa Rita)', dataAnamnese:'2024-04-08'},
  {nome:'Edna Pereira', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'A definir', email:'edanamaria17@gmail.com', telefone:'3212084274', piramide:'1 e 3', objetivo:'Saúde', restricoes:'Não lembro', academia:'Nem uma', dataAnamnese:'2024-04-09'},
  {nome:'laura Basso', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'laurabasso101@gmail.com', telefone:'51980132544', piramide:'1-bumbim 2-barriga 3-coxa  4-costas', objetivo:'Fazer os exercícios corretamente e ganhar massa muscular', restricoes:'nao tenho', academia:'performance', dataAnamnese:'2024-04-16'},
  {nome:'Lugar preferido?', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'pamella.correaft@gmai.com', telefone:'+1 561 929 0601', piramide:'1 barriga; 2 coxa, 3 bumbum, 4 braço', objetivo:'Emagrecer', restricoes:'Nenhuma relatada', academia:'YouFit', dataAnamnese:'2024-04-18'},
  {nome:'Claudia correa', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Intermediário', freq:'4x por semana', email:'cld_maria@hotmail.com', telefone:'7742970981', piramide:'1 2,3,4,', objetivo:'Saúde definição', restricoes:'Pescoço', academia:'Planets fitness', dataAnamnese:'2024-05-01'},
  {nome:'Schariana Larrea', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'4x por semana', email:'schariana.grandini@gmail.com', telefone:'51989175126', piramide:'3', objetivo:'Exercício corretos e ter definição muscular', restricoes:'Nenhuma relatada', academia:'Yes fitness', dataAnamnese:'2024-06-10'},
  {nome:'Fabiana Azambuja dos Santos Barbosa', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'6x por semana', email:'pytty.faby@gmail.com', telefone:'51995452270', piramide:'1-barriga- 2 pernas 3- bracos 4- bumbum', objetivo:'Perder gordura e aumentar a massa magra', restricoes:'Nenhuma relatada', academia:'Em casa', dataAnamnese:'2024-06-23'},
  {nome:'Giselle Troglio', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'gitroglio@hotmail.com', telefone:'51999869394', piramide:'Perna, barriga, costas, abdômen, braco', objetivo:'Ganhar massa muscular e perder gordura', restricoes:'Nenhuma relatada', academia:'Moinhos', dataAnamnese:'2024-06-26'},
  {nome:'Kimberlyn', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'A definir', email:'kimberlynvitoriamoura@gmail.com', telefone:'51986108215', piramide:'1- barriga  2- bumbum 3- coxa 4- costas', objetivo:'É perder a gordura localizada da minha barriga', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2024-07-16'},
  {nome:'Luana Cardoso Borges', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'4x por semana', email:'luannacardoso275@gmail.com', telefone:'51996374480', piramide:'Barriga - costas - bumbum - coxa', objetivo:'Meu objetivo é principalmente diminuir a barriga que me incomoda bastante e deixar o bumbum mais durinho', restricoes:'Nenhuma relatada', academia:'Rt fitness jardim dos lagos', dataAnamnese:'2024-07-18'},
  {nome:'Stephani Azevedo Araujo', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'5x por semana', email:'stephaniaraujo.2000@gmail.com', telefone:'51997308801', piramide:'1° Glúteos  2° Perna  3° barriga  4° costas/ parte superior no geral', objetivo:'definir o corpo', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2024-07-19'},
  {nome:'Laís Silva Peixoto', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'laispeixotosilva123@gmail.com', telefone:'51993921086', piramide:'1- coxa 2 bumbum- barriga 4 braços', objetivo:'Preciso de treinos específicos, para o pouco que eu posso treinar que é 3 dias sejam bem aplicados. Tenho uma rotina corrida, filho de 1.8 meses então tudo precisa ser otimizado', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2024-07-30'},
  {nome:'Flavia', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'fs456651@gmail.com', telefone:'2027517987', piramide:'2', objetivo:'Emagrecer e tirar a flacidez das pernas e dos braços', restricoes:'Nenhuma relatada', academia:'La fitness', dataAnamnese:'2024-08-02'},
  {nome:'Caren Batista', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'cb030885@gmail.com', telefone:'51998503010', piramide:'1- bumbum 2- coxa 3-barriga 4-costas', objetivo:'Emagrecimento e definição.', restricoes:'Sim, desgaste na L5 (coluna); desgaste na L5 (coluna), não tenho exames recentes.', academia:'Studio Training', dataAnamnese:'2024-08-05'},
  {nome:'Renata Jaqueline Sampaio', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'rjaquesampaio@gmail.com', telefone:'51996038856', piramide:'1- 3- 2-4', objetivo:'Glúteos ….. emagrecimento e ganho massa muscular', restricoes:'Tenho tendinite no joelho direito, normalmente sinto dor na canela ao fazer esteira', academia:'Engenharia do Corpo', dataAnamnese:'2024-08-05'},
  {nome:'Isnelen Piacini', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Intermediário', freq:'3x por semana', email:'isnelenpiacini06@gmail.com', telefone:'51993072923', piramide:'1° coxas, 2° bumbum, 3° Abdômen, 4° braços', objetivo:'Perda de peso e tonificação', restricoes:'Nenhuma relatada', academia:'No parque esportivo da Puc', dataAnamnese:'2024-08-12'},
  {nome:'Ana Lúcia da rosa', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'5x por semana', email:'darosaana799@gmail.com', telefone:'51 98186-2058', piramide:'1', objetivo:'Secar e definir meu corpo', restricoes:'Nenhuma relatada', academia:'Star fit', dataAnamnese:'2024-08-13'},
  {nome:'Guilherme L Souza', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'A definir', email:'guilibretti@gmail.com', telefone:'051995229547', piramide:'1- Braço completo (tríceps, bíceps e ante braço) 2- Perna completa 3- Ombros 4- Abdômen', objetivo:'Adquirir um shape com volume e seco', restricoes:'Não.; Não.; Não.', academia:'Performance Residencial', dataAnamnese:'2024-08-14'},
  {nome:'Maria Eduarda Guimarães de Souza', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'mariasouzaguimaraes2002@gmail.com', telefone:'51999070872', piramide:'1 bumbum 2 quadríceps 3 abdômen 4 braço', objetivo:'Emagrecer', restricoes:'Escoliose', academia:'Corpo e forma', dataAnamnese:'2024-08-14'},
  {nome:'Nathalia Gonçalves San Martin', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'6x por semana', email:'natthaliag21@gmail.com', telefone:'51994239423', piramide:'Glúteo, coxa, barriga e costas', objetivo:'Definir o corpo', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2024-08-14'},
  {nome:'Fabiana Peres Trindade', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'fabiana_perestrindade@hotmail.com', telefone:'51980457289', piramide:'Na real eu preciso de força muscular e emagrecer', objetivo:'Focar na academia', restricoes:'Bursite; Bha as águas levaram kkk', academia:'Atualmente na usina', dataAnamnese:'2024-08-16'},
  {nome:'Felipe Carneiro de Araújo', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'fca.carneiro03@gmail.com', telefone:'48996015089', piramide:'1- peito , 2 - barriga , 3 - biceps , 4 - coxa', objetivo:'Emagrecer e definir', restricoes:'Nenhuma relatada', academia:'Engenharia do Corpo', dataAnamnese:'2024-08-19'},
  {nome:'Rafaela Guimarães', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Intermediário', freq:'5x por semana', email:'rafaela.pguimaraess@gmail.com', telefone:'5199272-6842', piramide:'2 1 3 4', objetivo:'Emagrecer e definir', restricoes:'Nenhuma relatada', academia:'Yes ou Performance', dataAnamnese:'2024-08-19'},
  {nome:'Tatiane da Costa Caon', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'A definir', email:'tatianecc007@gmail.com', telefone:'51985621412', piramide:'1- 3 - 2 -4', objetivo:'Perder medidas e definir', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2024-08-20'},
  {nome:'Jessika cunha', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'A definir', email:'jeekcunha@gmail.com', telefone:'51 996547272', piramide:'Glúteos, coxas, barriga e costas', objetivo:'Me auxiliar nos treinos', restricoes:'Apenas vertigem, tontura, quando faço algum exercício de subida e descida, ou algo muito rápido!', academia:'Em casa', dataAnamnese:'2024-08-21'},
  {nome:'Francely Andrades Waszak', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Intermediário', freq:'5x por semana', email:'francely.waszak@gmail.com', telefone:'51992952415', piramide:'1 barriga 2 perna 3 bumbum 4 peito', objetivo:'Perca de peso, definição muscular, bem estar', restricoes:'Desgaste de cartilagem no joelho esquerdo.', academia:'Performace', dataAnamnese:'2024-08-24'},
  {nome:'gabriely waszak souza', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'5x por semana', email:'gabriely.waszak@icloud.com', telefone:'51992070541', piramide:'barriga', objetivo:'emagrecimento', restricoes:'Nenhuma relatada', academia:'performance', dataAnamnese:'2024-08-24'},
  {nome:'Priscila Ambos', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'prisilambos@gmail.com', telefone:'51 98266-0725', piramide:'1-bumbum urgente  2 coxas urgente   3-barriga urgente  4 - braços', objetivo:'Depois que eu perdi 22 kg eu tô.com as pernas finas e o bumbum caído isso me encomenda  muito fora a barriga  os braços  gordinhos ainda mais não quero emagrecer  quero deixar  essas partes do meu corpo melhores bumbum empinado  barriga com.menos gordura  braços  também  e coxas mais grossas  não estou feliz com meu corpo tudo fica feio', restricoes:'Não tenho nada som preguiça', academia:'Ainda nao sei preciso  de indicação  nao consigo  me adaptar  e tenho vergonha  de ir pra academia', dataAnamnese:'2024-09-07'},
  {nome:'Giovana mazuhim', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'5x por semana', email:'giovanathamy@gmail.com', telefone:'51986592605', piramide:'Todos', objetivo:'Consiga atingir o máximo dos meus objetivos', restricoes:'Tenho o menisco discoide; Não enchente levou', academia:'Performance', dataAnamnese:'2024-09-07'},
  {nome:'Paula Oliveira', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'5x por semana', email:'paula95817338@gmail.com', telefone:'51997762225', piramide:'1-barriga 2bumbum 3coxa 4costas', objetivo:'Sair da zona de conforto, parar de treinar fofo e pegar pesado', restricoes:'Nenhuma relatada', academia:'Talvez na yes fitness em eldorado, vou sair da academia que estou em Guaíba.', dataAnamnese:'2024-09-07'},
  {nome:'Eduarda Machado Coutinho Gadêa', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'4x por semana', email:'dudamachado00@outlook.com', telefone:'51999381049', piramide:'1 bumbum, 2 coxas, 3 barriga e 4 braços', objetivo:'Ganhar massa muscular', restricoes:'Escoliose lombar', academia:'Academia MOVE EXTREME JURERÊ', dataAnamnese:'2024-09-08'},
  {nome:'Carolina Machado Vargas', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'carolinamachadovargas@gmail.com', telefone:'51992391295', piramide:'1 Barriga 2 bumbum 3 coxa 4 costas e reforço muscular devido a minha lesão por causa da ginástica artística como já fizemos em 2019 nos treinos', objetivo:'Tirar as minhas dores da cervical, lombar, e joelho, fazer o reforço muscular, emagrecer, perder a barriga e definir o corpo', restricoes:'Sim na coluna compressão disco, tendinite braço direito, lesão cervical perda de força braço direito e encurtamento perna esquerda devido a lesão fêmur....Nariga tinha treino específico pra isso em 2019; Tenho mas não tenho mais os exames por causa da enchente; As questões da coluna', academia:'Acredito que vou alternar academia e treino em casa ou em parque/ caminhada ao ar livre depende mto do meu tempo/ dia', dataAnamnese:'2024-09-09'},
  {nome:'Thalia', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'kohlsthalia@gmail.com', telefone:'51980490716', piramide:'1- barriga  2-bumbum  3-coxas  4- costas', objetivo:'Me ajudar a alcançar os objetivos traçados de emagrecimento e definição', restricoes:'Somente dores na lombar; Tenho', academia:'Na Yes fitnes por enquanto', dataAnamnese:'2024-09-16'},
  {nome:'Jenifer dos Santos', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'jenifer_dro@hotmail.com', telefone:'51984949198', piramide:'1- Bumbum 2- Pernas 3- Barriga 4- Costas', objetivo:'Emagrecer, criar bumbum e perna e definir a bartiga', restricoes:'Não tenho e nunca tive', academia:'Acredito que na Yes', dataAnamnese:'2024-09-30'},
  {nome:'Aline Ribeiro Garcias', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'alinematosgrcias@gmail.com', telefone:'51992756129', piramide:'1 e 3', objetivo:'Emagrecer ,pretendo ficar com uns 59 k,afinar cintura,definir pernas ,mas n quero engrossar mais,bumbum só definir e empinar ,tmb n quero aumentar bumbum', restricoes:'Nenhuma relatada', academia:'Sencefit', dataAnamnese:'2024-10-03'},
  {nome:'Angelina Paz Mendes', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'A definir', email:'angelinatst@gmail.com', telefone:'51985659734', piramide:'1 - costas  2 - braços  3 - glúteos  4 - coxas', objetivo:'Emagrecimento e definição muscular', restricoes:'Nenhuma relatada', academia:'Busata', dataAnamnese:'2024-10-09'},
  {nome:'Renata Pedrotti Franco', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'5x por semana', email:'renata.pedrotti@hotmail.com', telefone:'51989105995', piramide:'1 costas,2ombro e bíceps, 3 quadríceps 4 gluteo', objetivo:'Emagrecimento e definição muscular', restricoes:'Nenhuma relatada', academia:'Engenharia do corpo', dataAnamnese:'2024-10-16'},
  {nome:'Ester correa zanotelli', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'ester.zanotelli@gmail.com', telefone:'51 997216657', piramide:'1 barriga, 2 bumbum, 3 coxa 4 peitoral', objetivo:'Direcionar para que eu faça exercícios corretamente para poder atingir com eficácia meus objetivos.', restricoes:'Eu sinto dor no meu joelho direito que preciso investigar.', academia:'Performance', dataAnamnese:'2024-10-21'},
  {nome:'Juliano Pinto Mello', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'4x por semana', email:'juliano_mello@hotmail.com', telefone:'(55) 99664-4494', piramide:'Costas', objetivo:'Em um momento inicial, emagrecimento. Depois hipertrofia', restricoes:'Nenhuma relatada', academia:'TopOne Jardim Botânico', dataAnamnese:'2024-10-23'},
  {nome:'Priscila Ribeiro Santos', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'2x por semana', email:'prytchy@hotmail.com', telefone:'51984816358', piramide:'3 -4- 2- 1', objetivo:'Eliminar peso e ganhar massa muscular', restricoes:'Já quebrei o 5º metatarso do pé direito e já tive uma torção no pé esquerdo', academia:'Engenharia do Corpo - São Leopoldo', dataAnamnese:'2024-10-28'},
  {nome:'JEFERSON RODRIGUES BARBOSA', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'6x por semana', email:'Jefersonbarbosa1993@hotmail.com', telefone:'51 986600078', piramide:'Barriga, costas, braços, pernas para corrida', objetivo:'Aprender a sair do plano feito pela acadêmia', restricoes:'Ondo', academia:'Cia do corpo', dataAnamnese:'2024-11-05'},
  {nome:'Ana Carolina Pereira Fedatto', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'6x por semana', email:'anacarolinapereirafedatto@gmail.com', telefone:'51996452269', piramide:'1-3-2-4', objetivo:'Me preparar para o teste de aptidao fisica, ganhar resistência e massa muscular', restricoes:'sim, no ligamento do dedao do pe direito', academia:'Performance Fitness', dataAnamnese:'2024-11-11'},
  {nome:'Fernando kunzler', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'Fernandokunzler99@gmail.com', telefone:'51999860995', piramide:'Ombros e peito', objetivo:'Perder gordura e ganhar massa magra', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2024-11-14'},
  {nome:'Rafael Brito Feck', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'5x por semana', email:'rafaelbfeck@gmail.com', telefone:'51980114807', piramide:'Barriga, peitoral, bicips e costas', objetivo:'Emagrecimento/hipertrofia', restricoes:'Nenhuma relatada', academia:'Usina do corpo', dataAnamnese:'2024-11-14'},
  {nome:'Renata Rolim', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Intermediário', freq:'4x por semana', email:'contato.renatarolim@gmail.com', telefone:'55 98153-7900', piramide:'1 bumbum 2 barriga 3 ombro 4 costas', objetivo:'Quero perder gordura e ganhar massa magra, melhorar o treino pra atingir mais rapido o objetivo', restricoes:'Realizando o exercício na cadeira adutora, senti um estalo na junção dos ossos púbicos além de dor. Fiquei sem fazer o exercício por 2 semanas e quando voltei a fazer, iniciei com peso bem baixo por medo de me machucar novamente.; Tenho 26 graus de escoliose rotatória em C. Pulso esquerdo tem um cisto e impossibilita de realizar atividades que precise do apoio das mãos. Principalmente quando o braço e a mão formam um ângulo de 90 graus (por exemplo, ao fazer flexao)', academia:'Perfil (em gravatai)', dataAnamnese:'2024-11-15'},
  {nome:'nicolsa batista fernandes dos santos', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'nicolasbckp612@Gmail.com', telefone:'51986136039', piramide:'1 - Barriga 2 - Coxas 3 - Costas 4 - Bumbum', objetivo:'Conseguir ter uma definição', restricoes:'Nenhuma relatada', academia:'Unisa do Corpo - Centro Historico', dataAnamnese:'2024-11-17'},
  {nome:'Micaela de Souza Santos', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'2x por semana', email:'souza-micaela@outlook.com', telefone:'51980297897', piramide:'Barriga, costas, busto, bumbum', objetivo:'Melhor disposição física. Redução de medidas', restricoes:'Nunca tive', academia:'Ideal - Guaíba bairro santa rita', dataAnamnese:'2024-11-18'},
  {nome:'Luisa Camila Buchert Moser', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'3x por semana', email:'luisacbmoser@hotmail.com', telefone:'51999931612', piramide:'1 barriga 2 bumbum 3 pernas', objetivo:'Definição e crescimento', restricoes:'Nenhuma relatada', academia:'Yes fitness', dataAnamnese:'2024-11-21'},
  {nome:'Carla Simone Viafore', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'3x por semana', email:'carlasviafore@gmail.com', telefone:'51992116538', piramide:'Barriga, bumbum, braços e coxas', objetivo:'Emagrecer e ganhar massa muscular', restricoes:'joelho', academia:'Usina do corpo', dataAnamnese:'2024-11-25'},
  {nome:'Denilson Bitencourt', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'5x por semana', email:'denilsonnasbit23@gmail.com', telefone:'51982074359', piramide:'1- barriga (concentração de gordura)  2- glúteo (concentração de gordura) 3- costas 4- Coxa', objetivo:'- Atingir e manter o peso entre 85/87kg  - Definição abdominal - Melhorar meu desempenho nos esporte', restricoes:'Sim, desgaste nos discos da coluna.Tive nesse ano fratura e deslocamente em um dedo do pé, mas hoje já não influencia em movimento e treinos; Sim, Lordose lombar, e desgaste de alguns discos.', academia:'Em casa, tenho alguns equipamentos e da para trabalhar todos os musculos, somente perna que faço um trabalho maior com o próprio peso do corpo', dataAnamnese:'2024-11-26'},
  {nome:'Yasmin lara Souza gutheil', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'yasmimlara12345@gmail.com', telefone:'51992353733', piramide:'1- bumbum 2- coxa 3-barriga 4-braços', objetivo:'Secar a barriga e definir os músculos', restricoes:'Não tenho', academia:'Sesc', dataAnamnese:'2024-12-02'},
  {nome:'Vinicius Bittencourt', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'vinicius.m.bittencourt@hotmail.com', telefone:'51985692526', piramide:'Não tenho um específico', objetivo:'Emagrecimento e definição', restricoes:'Fratura nos dois braços', academia:'Do condomínio', dataAnamnese:'2024-12-06'},
  {nome:'Ednaldo', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'3x por semana', email:'edh.protec@hotmail.com', telefone:'7745783341', piramide:'1234', objetivo:'Melhor postura, fortalecimento geral e melhor disposição', restricoes:'Nenhuma relatada', academia:'', dataAnamnese:'2024-12-07'},
  {nome:'Tais Lavarda', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'5x por semana', email:'angela.lavarda97@gmail.com', telefone:'51994280158', piramide:'Barriga, braços, bunda e coxa', objetivo:'Perder gordura', restricoes:'Tenho o ligamento do lado de dentro do joelho direito inflamado, já fiz ecografia e consultas', academia:'Yes fitness', dataAnamnese:'2024-12-07'},
  {nome:'Jêniffer Tavares Zeferino', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'4x por semana', email:'jeniffer.poa@hotmail.com', telefone:'51989491930', piramide:'1 membros inferiores 2 barriga 3 bumbum 4 costas', objetivo:'Melhorar o condicionamento físico e aumento de massa magra.', restricoes:'Sim. Tenho pés cavos, no qual tenho limitações: como por exemplo exercícios de impacto: correr.; Sim, cirurgia dos 6 aos 13 anos para correção dos pés cavos.', academia:'Academia condomínio', dataAnamnese:'2024-12-12'},
  {nome:'Manassés Rafael dos Santos Torales', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'5x por semana', email:'manassestorales@gmail.com', telefone:'51984239326', piramide:'3,4,2,1', objetivo:'Emagrecer, ganhar massa muscular', restricoes:'Nenhuma relatada', academia:'Smartfit', dataAnamnese:'2024-12-14'},
  {nome:'Pedro Henrique Mendes Geremias Moreira', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'pedromendes.drope@gmail.com', telefone:'51998501796', piramide:'1° barriga 2° braço 3° perna 4° costas', objetivo:'Emagrecer e definir', restricoes:'Faz uns 4 meses quebrei a cabeça do radio do braço esquerdo, próximo ao cotovelo', academia:'Ac fitnes', dataAnamnese:'2024-12-14'},
  {nome:'Adriane Lopes Hermel', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'4x por semana', email:'Adriane.hermel@gmail.com', telefone:'51995744214', piramide:'1- bumbum,2-coxa, 3- barriga, 4- costas', objetivo:'Ter os exercícios corretos pra certinho e ter um bom resultado', restricoes:'Nenhuma relatada', academia:'Engenharia do corpo ou moinhos', dataAnamnese:'2025-01-29'},
  {nome:'Felipe Motta batista', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'A definir', email:'felipemottabatista@gmail.com', telefone:'51996957053', piramide:'3', objetivo:'Emagrecer e ganhar músculo', restricoes:'Nenhuma relatada', academia:'Em casa', dataAnamnese:'2025-02-10'},
  {nome:'Caroline Pilastro', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Intermediário', freq:'A definir', email:'carolinepilastro@outlook.com', telefone:'51990121794', piramide:'1 3 4 2', objetivo:'Alcançar meu objetivo com o foco personalizado', restricoes:'Não tenho, porem no periodo de colicas menstruais preferia fazer apenas superiores ou pesar mais no superior e menos inferior', academia:'Não treino', dataAnamnese:'2025-02-17'},
  {nome:'Sindel piacini', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'sindelpiacini02@gmail.com', telefone:'51995806920', piramide:'Glúteo, costas, pernas, braços', objetivo:'Fortalecimento muscular.', restricoes:'Não.; Escoliose, mt dor ombros e lombar.', academia:'Smart Fit', dataAnamnese:'2025-02-21'},
  {nome:'Daniel Valenzuela', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'4x por semana', email:'dazanval10@gmail.com', telefone:'4704030613', piramide:'1) pernas, 2- barriga 3) ombros 4) braços', objetivo:'Hipertrofia e uma rotina de treino balanceada e eficiente', restricoes:'Nenhuma relatada', academia:'LA Fitness', dataAnamnese:'2025-02-21'},
  {nome:'Larissa Santana', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'larissa.rsantana@gmail.com', telefone:'51992704595', piramide:'1-  bumbum, 2- barriga, 3- coxa, 4- braços', objetivo:'Definir e crescer. Sempre treinei e vi resultado apenas quando estava foçada de verdade e pegando bastante peso, agora que sei que consigo preciso de uma rotina mais regrada.  Meu foco é o mesmo de quase toda mulher, crescer glúteo e pernas, diminuir a barriga, definir superiores', restricoes:'Não que eu lembre', academia:'Performance', dataAnamnese:'2025-02-25'},
  {nome:'André Vitor Padilha da Silva', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'a.padilhasilva@gmail.com', telefone:'53984568409', piramide:'Parelho', objetivo:'Perda de gordura, ganho de massa muscular', restricoes:'Sim, deslocamento patelar nos dois joelhos, patela rasa.; Patela rasa; Sim, dois joelhos', academia:'Engenharia do corpo', dataAnamnese:'2025-02-25'},
  {nome:'Luiza Gabriella', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'6x por semana', email:'lugabi0804@gmail.com', telefone:'51992079874', piramide:'1 bunda, 2 quadríceps, 3 barriga e cintura, 4 costas e braços', objetivo:'Chegar nos meus objetivos com treinos feitos pra mim. Ter auxílios em algumas dúvidas que eu não possa responder e entender um pouco mais sobre como funciona a transformação do corpo.', restricoes:'No meu primeiro dia de academia, fiz algo de errado, e no segundo treino depois de voltar pra casa, apareceu os sintomas de uma contratura na panturrilha. (Isso faz 1 mês) Repousei na época por uma semana praticamente, e depois continuei. Não conseguia andar normal.    Na enchente de 2023 já cai e bati o joelho bem forte no chão também, então talvez em algum momento tenha que fortalecer essas áreas.; Não.', academia:'Performance', dataAnamnese:'2025-03-02'},
  {nome:'Camila Figueiredo Lemos', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'cami.lemosfl@gmail.com', telefone:'51 98617-0882', piramide:'1, 2, 3 e 4', objetivo:'Perda de peso.', restricoes:'Não.; Não.', academia:'.', dataAnamnese:'2025-03-07'},
  {nome:'Gabriela fortes Cabral dos santos', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'gabrielacabral229@gmail.com', telefone:'51990143996', piramide:'1-bumbum 2-barriga 3- coxa 4- costas', objetivo:'Corpo definido', restricoes:'Nenhuma relatada', academia:'Yes fitness', dataAnamnese:'2025-03-12'},
  {nome:'Aline da silva', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Intermediário', freq:'A definir', email:'aline_nathally@hotmail.com', telefone:'9547085856', piramide:'1. 2. 3. 4', objetivo:'Criar um corpo lindo e saudável', restricoes:'Nenhuma relatada', academia:'Lá fitness', dataAnamnese:'2025-03-23'},
  {nome:'joao carlos lemos de souza filho', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'5x por semana', email:'joao.contabil@yahoo.com.br', telefone:'51989502000', piramide:'1 abdômen 2 peito 3 costas  4 pernas', objetivo:'Fica igual ao ramon dino', restricoes:'Mao; Mao', academia:'Ctpeamce', dataAnamnese:'2025-04-05'},
  {nome:'Renata', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'3x por semana', email:'renatalopes2530@gmail.com', telefone:'51998064208', piramide:'1º Barriga, 2º pernas,3º coxa e 4º costas', objetivo:'Perder peso, perder barriga principalmente e ganhar mais pernas e bumbum', restricoes:'Dor no joelho direito , ele é para dentro e sinto que se Faço leg puxa pra dentro; Só esse detalhe no joelho parece que puxo para dentro quando faço leg ou agachamento', academia:'Ideia fitness', dataAnamnese:'2025-04-09'},
  {nome:'Ana Paula Furini', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'3x por semana', email:'paula_furini@hotmail.com', telefone:'51 984846432', piramide:'1-coxa, 2-abdômen, 3-gluteo, 4-biceps', objetivo:'Definição muscular e hipertrofia', restricoes:'Desloquei uma vértebra da lombar, normalmente não sinto dores ao praticar exercícios, mas cuido bastante ao aumentar a carga no treino das pernas pq forço e fica incomodando as vezes', academia:'Perfect da assis brasil', dataAnamnese:'2025-04-28'},
  {nome:'Fernando Pereira', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'5x por semana', email:'fernandosilp1@gmail.com', telefone:'51998163667', piramide:'Abdome, ombros, panturrilha, costas', objetivo:'Corrigur os níveis de musculatura corporal em ambos os lados do corpo e ter músculos mais estéticos.', restricoes:'Lombar: Nada grave, mas sinto pressão na lombar fazendo agachamento com barra, sumô e encolhimento de ombros. Ombros: Sinto ombro ao fazer supino com barra, prefiro sempre com alteres.; Não.; Não.', academia:'Perfect', dataAnamnese:'2025-04-28'},
  {nome:'Graziele Stefanno', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'4x por semana', email:'graziele.moura.stefanno@gmail.com', telefone:'55996889180', piramide:'1 coxa, 2 barriga, 3 bumbum e 4 ombro, biceps, costas e  triceps', objetivo:'Perder gordura', restricoes:'Não! De vez em quando sinto dor no joelho esquerdo', academia:'Smartfit', dataAnamnese:'2025-05-12'},
  {nome:'Diego Almir da Trindade Machado', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'3x por semana', email:'diego.trindade.machado@gmail.com', telefone:'51 991712262', piramide:'3, 4, 2, 1', objetivo:'Perder gordura', restricoes:'Leve no tendão de Aquiles; Não! Apenas uma leve dificuldade de mobilidade.', academia:'ALL Fit ou 26 Fit (pelo Gym Pass)', dataAnamnese:'2025-05-13'},
  {nome:'Aline Gomes', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'linygomes@gmail.com', telefone:'51993473308', piramide:'barriga, bumbum, coxa, braços', objetivo:'emagrecer', restricoes:'Nenhuma relatada', academia:'Allpfit', dataAnamnese:'2025-05-14'},
  {nome:'Camila Couto Frazão', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Intermediário', freq:'6x por semana', email:'ccoutofrazao@gmail.com', telefone:'(51) 9 8973-2353', piramide:'1- glúteo  2- coxa 3- posterior  4- costas', objetivo:'Perder peso q ganhei na gestação e melhorar a aparência dos músculos', restricoes:'Nenhuma relatada', academia:'CT performance', dataAnamnese:'2025-05-20'},
  {nome:'Paula Schmitt', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'5x por semana', email:'paulaschmittc@gmail.com', telefone:'+351 967981658', piramide:'1- coxa;  2- bumbum;  3- barriga;  4- costas;', objetivo:'Ganhar inferiores: meu corpo é em formato de triângulo invertido, meu objetivo é deixar mais harmônico.', restricoes:'Sim, hipertensão patelar no joelho esquerdo, estou tomando medicação por 60 dias. Acaba final de junho. Rantudil 90 retard. Em pouco tempo (1 ano) aumentei muito o peso e sobrecarregou meu joelho esquerdo, que é a perna mais fraca.; Sim, não posso treinar com muito peso, nem corrida ou impactos.', academia:'Uma academia de bairro', dataAnamnese:'2025-06-02'},
  {nome:'Barbara furnari', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'Barbaraalbieri@hotmail.com', telefone:'±14074856686', piramide:'Barriga', objetivo:'Perder peso', restricoes:'Nenhuma relatada', academia:'YMCA dr Philips', dataAnamnese:'2025-06-11'},
  {nome:'Rafaela', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'rafaelax3@live.com', telefone:'7742255608', piramide:'3', objetivo:'Weight loss', restricoes:'No; No; No', academia:'Planet fitness', dataAnamnese:'2025-06-12'},
  {nome:'Íris Fernanda Kessa de Jesus', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'iris-fernanda122011@hotmail.com', telefone:'17991789886', piramide:'1- barriga, 2- coxas, 3- bumbum, 4- costas', objetivo:'Emagrecer', restricoes:'Nenhuma relatada', academia:'Treinar em casa', dataAnamnese:'2025-06-14'},
  {nome:'Kamila Milena Moura da Silva', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'5x por semana', email:'kamilamoura98@hotmail.com', telefone:'51984502809', piramide:'barriga - 1 bumbum - 2 coxa - 3 costas - 4', objetivo:'emagrecimento e definição', restricoes:'Nenhuma relatada', academia:'Yes fitness', dataAnamnese:'2025-06-16'},
  {nome:'Mariana Gonçalves Soares', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Intermediário', freq:'4x por semana', email:'soaresgmari.12@gmail.com', telefone:'9785309369', piramide:'Barriga, coxa, bumbum, costas', objetivo:'Alcançar o corpo dos meus sonhos', restricoes:'Nenhuma relatada', academia:'Planet fitness e em casa', dataAnamnese:'2025-06-17'},
  {nome:'Fernando Soares', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'soaresfernando024@gmail.com', telefone:'51985006949', piramide:'Barriga, peito, braços e costas', objetivo:'Perder peso', restricoes:'Nenhuma relatada', academia:'Ainda estudando locais', dataAnamnese:'2025-07-18'},
  {nome:'Fabiane severo de oliveira', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'4x por semana', email:'fabiane_severo@outlook.com', telefone:'51-992635974', piramide:'Barriga, costas (reduzir as bordas de catupiry), bumbum, braços e coxas', objetivo:'Emagrecer', restricoes:'Cesárea há 4 meses', academia:'Em casa.', dataAnamnese:'2025-07-22'},
  {nome:'Tamia Cunha', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Intermediário', freq:'4x por semana', email:'lolocunha38@gmail.com', telefone:'(51)996449210', piramide:'1,3,4,2', objetivo:'Fortalecimento muscular', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2025-08-07'},
  {nome:'Andrieli Laux', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'5x por semana', email:'andrieli.lauxx@gmail.com', telefone:'51997972100', piramide:'1- glúteo  2- quadríceps  3-ombro e costas  4- barriga', objetivo:'Emagrecimento é definição', restricoes:'Nenhuma relatada', academia:'CT commando (ao lado da minha casa)', dataAnamnese:'2025-08-07'},
  {nome:'Lisandra Bernardy', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'lili.bernardy@gmail.com', telefone:'51997889980', piramide:'1º barriga  2º braços  3º glúteos  4º coxa', objetivo:'Perda de peso', restricoes:'Tive fratura na tíbia', academia:'CTperformance - Eldorado do Sul', dataAnamnese:'2025-08-07'},
  {nome:'Sofia Bamberg Cardozo dos Santos', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'5x por semana', email:'sofibamberg@gmail.com', telefone:'51985464624', piramide:'1 bumbum (glúteo médio principalmente, não gosto do meu glúteo de lado) 2cintura  3quadriceps  4costas  Quero braços definidos também', objetivo:'Conseguir chegar em uma meta de estética de corpo', restricoes:'Tenho problema na canela por conta do futvôlei, mas maior maior dificuldade é em corrida', academia:'Smart Fit Shopping Walling', dataAnamnese:'2025-08-07'},
  {nome:'Eduardo Denti', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'3x por semana', email:'eng.eduardodenti@gmail.com', telefone:'51981151411', piramide:'1 . Costas 2. Ombros 3. Peito 4. Abdomen', objetivo:'Estética', restricoes:'Nenhuma relatada', academia:'Do meu condominio', dataAnamnese:'2025-08-07'},
  {nome:'Bianca', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'biancasilveirars22@gmail.com.br', telefone:'51995219092', piramide:'3- 2-1-4 coloquei assim mas todas estão em primeiro', objetivo:'Perca de gordura e ganhar musculo', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2025-08-07'},
  {nome:'Bruna Bruniszaki', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'A definir', email:'bruniszakibru@gmail.com', telefone:'51995419409', piramide:'1costas, 2 bracos, 3 pernas, 4 abdomen', objetivo:'Acelerar a queima', restricoes:'Tenho síndrome do túnel do carpo', academia:'Performance ( sans souci )', dataAnamnese:'2025-08-09'},
  {nome:'Victoria Medeiros Dorneles', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'medeirosdornelesv@gmail.com', telefone:'51983526469', piramide:'3 - 2 - 1 - 4', objetivo:'Emagrecimento e ganho de massa magra (Fortalecimento tb para ter mais desenvoltura no Cross)', restricoes:'Ano passado tive uma lesão no pescoço (torcicolo) fazendo um movimento no Crossfit.', academia:'Academia Perfect', dataAnamnese:'2025-08-12'},
  {nome:'saimon guevara brum', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'saimonbrum@gmail.com', telefone:'51991397251', piramide:'1 costas 2 braço 3 coxa 4 abdomen', objetivo:'Emagrecimento é definicao', restricoes:'Dedo médio da mão direita mas não senti alterações', academia:'Perfect', dataAnamnese:'2025-08-15'},
  {nome:'Mauro', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'5x por semana', email:'mauro_junior1@hotmail.com', telefone:'51 999636817', piramide:'1- braços 2 - pernas 3 - peito 4 - costas', objetivo:'Emagrecimento e ganho de massa muscular', restricoes:'Ruptura do reto femural e ligamentos de tornozelo', academia:'Engenharia do corpo', dataAnamnese:'2025-08-19'},
  {nome:'Angélica Nobre', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'4x por semana', email:'nobangelica2019@gmail.com', telefone:'51989164716', piramide:'1- Barriga 2- costas 3- braços  4- bumbum', objetivo:'Emagrecer e fortalecer músculos', restricoes:'Sim, trinquei o cóccix há mais de 10 anos.', academia:'Sani academia', dataAnamnese:'2025-08-20'},
  {nome:'Liane Cardoso guilherme', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'lcguilherme90@gmail.com', telefone:'51994661446', piramide:'1 - barriga, 2 - bumbum, 3 - costas e 4 - coxa', objetivo:'Emagrecimento e definição', restricoes:'Joelho', academia:'Não sei ainda', dataAnamnese:'2025-08-29'},
  {nome:'Kewelin Alba', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'4x por semana', email:'kewelinalba4@gmail.com', telefone:'51997340897', piramide:'1- Bumbum, 2- Barriga, 3- Coxa , 4- Costas', objetivo:'Desenvolver minha musculatura, gostar do que vejo no espelho.', restricoes:'Sim.; Sim. Ok, enviarei.; Não.', academia:'Perfect Academia Baltazar', dataAnamnese:'2025-08-31'},
  {nome:'Juliana Viana Andrade', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'juhvandrade@gmail.com', telefone:'51996328299', piramide:'costas, coxa, barriga e bumbum', objetivo:'resultado de definição', restricoes:'Nenhuma relatada', academia:'no litoral', dataAnamnese:'2025-09-01'},
  {nome:'Jennifer Araujo', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'5x por semana', email:'jenniferaraujos@gmail.com', telefone:'51984725089', piramide:'1- barriga  2- pernas 3- braços  4- bumbum', objetivo:'Definir meu corpo, ter uma rotina ativa e saudável.', restricoes:'Nenhuma relatada', academia:'Yes', dataAnamnese:'2025-09-04'},
  {nome:'Cindy Kittler', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'kittlercindy@gmail.com', telefone:'51991941952', piramide:'Barriga, costas, coxa, bumbum, (mas também adoro treino de ombro).', objetivo:'definir e perder gordura.', restricoes:'Tenho condromalácia no joelho direito. As vezes me dói ao treinar.', academia:'Rt Fitnees em Guaíba.', dataAnamnese:'2025-09-08'},
  {nome:'Gabriel Garcia', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'gabrielgarcias@live.com', telefone:'51996662407', piramide:'1 - peito  2 - ombro  3 - tríceps  4 - costa', objetivo:'treinos personalizados', restricoes:'tenho uma lesão no joelho no momento onde me impede exercícios de pernas, estou aguardando a data da cirurgia (janeiro) para retornar, por hora o médico indicou somente superiores, caminhada e bike; enviarei', academia:'engenharia do corpo', dataAnamnese:'2025-09-08'},
  {nome:'Jucelaine Antunes Simoes', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'jucesimoes@hotmail.com', telefone:'51984554782', piramide:'3-1-2-4', objetivo:'Conseguir me dedicar com ajuda aos treinos', restricoes:'Nenhuma relatada', academia:'Top1  e Coliseu', dataAnamnese:'2025-09-11'},
  {nome:'Vanessa Macedo Pacheco', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'nessamp10@hotmail.com', telefone:'51996598036', piramide:'Barriga, coxa, bumbum, braços', objetivo:'Emagrecer e fortalecer o corpo', restricoes:'Tenho dores no quadril, mas quando fazia atividade não sentia nada; Não até o momento', academia:'Pretendo na performance', dataAnamnese:'2025-09-15'},
  {nome:'Tarso marcuzzo', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'3x por semana', email:'tcmarcuzzo13@gmail.com', telefone:'55996751312', piramide:'Peitoral', objetivo:'Perder bf', restricoes:'Nenhuma relatada', academia:'Eng do corpo', dataAnamnese:'2025-09-18'},
  {nome:'Anna Caroliny Bordinhão Jorge', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'A definir', email:'anabordy29@gmail.com', telefone:'47997911252', piramide:'1. bumbum 2. coxa 3.  barriga 4. braços  5. costas', objetivo:'crescer', restricoes:'Nenhuma relatada', academia:'Roldão', dataAnamnese:'2025-09-25'},
  {nome:'Rodrigo da Cruz Lisboa', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'4x por semana', email:'lisboa_r@hotmail.com', telefone:'53981554957', piramide:'Perna, peito, braço e costas', objetivo:'Ajustar meus treinos na academia/reforço muscular', restricoes:'Nenhuma relatada', academia:'Moinhos fitness - menino deus', dataAnamnese:'2025-09-26'},
  {nome:'Luara', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'2x por semana', email:'luara.franceschini@yahoo.com.br', telefone:'51989099916', piramide:'1 barriga/2 pernas / 3 bumbun/ 4 braços', objetivo:'Emagrecer 12/15 kg e voltar minha alto estima', restricoes:'Nenhuma relatada', academia:'Ideia ou RT jardim dos lagos', dataAnamnese:'2025-09-29'},
  {nome:'Débora Rischtter', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'3x por semana', email:'debora.rischtter@gmail.com', telefone:'51 99102-4492', piramide:'3 - 1 - 2 - 4', objetivo:'Diminuir percentual gordura', restricoes:'Não lesão, mas senti a lombar no sumo terra e tive que reduzir carga e fortalecer.', academia:'WKT centro de treinamento', dataAnamnese:'2025-09-29'},
  {nome:'Luigi Canova', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'luigi.canova@gmail.com', telefone:'51 993598824', piramide:'Barriga, peito, costas, pernas', objetivo:'Emagrecimento junto com a dieta do Thales', restricoes:'Braço quebrado quando criança', academia:'Não sei', dataAnamnese:'2025-09-30'},
  {nome:'Bruna', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'3x por semana', email:'brunalarissafreire90@gmail.com', telefone:'38988167643', piramide:'Braços e Costa  Tem mais gordura acumulada)', objetivo:'Emagrecimento', restricoes:'Escoliose e Lordose', academia:'Em dúvida FULLTIME ou WCT', dataAnamnese:'2025-10-01'},
  {nome:'Rafaela Carvalho da Silveira', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'6x por semana', email:'raffasilveira_@hotmail.com', telefone:'5199346-8765', piramide:'Coxa, costas, bumbum, barriga', objetivo:'Definir mais o Corpo', restricoes:'Nenhuma relatada', academia:'Wave', dataAnamnese:'2025-10-06'},
  {nome:'Ariane Garcia', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'1x por semana', email:'ferraz-ariane@hotmail.com', telefone:'51989546730', piramide:'1- barriga 2- pernas 3-bracos 4-costa', objetivo:'Perder peso e criar uma rotina ativa de exercícios', restricoes:'Nenhuma relatada', academia:'Em casa', dataAnamnese:'2025-10-12'},
  {nome:'Flávia', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'4x por semana', email:'fonoflaviamachado@gmail.com', telefone:'51996355144', piramide:'coxa, costas, barriga, bumbum', objetivo:'Ganho de massa e definição', restricoes:'Nenhuma relatada', academia:'Olympo', dataAnamnese:'2025-10-15'},
  {nome:'Marciele Girelli', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'marcielegirelli3@gmail.com', telefone:'51996500970', piramide:'1bumbum ,3barriga,2coxa 4 costas', objetivo:'Perder barriga e ganhar massa magra', restricoes:'Já tive no joelho', academia:'Em Porto Alegre , não conheço academia ainda não sei quais os aparelhos ela tem , vou ir nela por ser do lado da empresa', dataAnamnese:'2025-10-15'},
  {nome:'Ariana Marques', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'5x por semana', email:'ariane-ashley@hotmaiil.com', telefone:'51985981461', piramide:'Abdômen e bumbum', objetivo:'Definir', restricoes:'Nenhuma relatada', academia:'Engenharia do corpo', dataAnamnese:'2025-10-16'},
  {nome:'Rafaella Martins Trindade', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'rafaellamtrindade@gmail.com', telefone:'51 996403997', piramide:'Barriga , bumbum ,coxa, costas', objetivo:'Resultado de um corpo moldado e fortalecido', restricoes:'Tornozelo nada grave', academia:'Performance', dataAnamnese:'2025-10-20'},
  {nome:'Luciane', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Intermediário', freq:'3x por semana', email:'apenasluh@hotmail.com', telefone:'51984503225', piramide:'1,2,3,4', objetivo:'Definição', restricoes:'Abdominoplastia', academia:'Ct performance', dataAnamnese:'2025-10-21'},
  {nome:'Lucianna Moraes', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'4x por semana', email:'luaparticheli@gmail.com', telefone:'(51)993866665', piramide:'Emagrecer, músculo venha depois. 3 e 4', objetivo:'Emagrecer', restricoes:'Sim, já fraturei o metatarso correndo na esteira quando fazia academia de segunda a sábado.', academia:'Smart Fit', dataAnamnese:'2025-10-29'},
  {nome:'Tania Maris Reus Salam von Saltiel', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Intermediário', freq:'3x por semana', email:'tania.salam@hotmail.com', telefone:'(51)992801119', piramide:'3', objetivo:'Perder peso', restricoes:'Fiz uma cirurgia de artrodese na lombar; Cirurgia coluna; Não posso posso realizar treino Legpress', academia:'Sesc Protasio Alves', dataAnamnese:'2025-11-07'},
  {nome:'Sabrina', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'3x por semana', email:'sabrinarockenbach.mkt@gmail.com', telefone:'51993555544', piramide:'1 - bumbum / 2 - cintura/barriga / 3 - costas. 4 - coxas', objetivo:'Afinar cintura e dar forma no bumbum.', restricoes:'Nenhuma relatada', academia:'Cia do corpo', dataAnamnese:'2025-11-08'},
  {nome:'Fernanda', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'fernandareichimbak@gmail.com', telefone:'51991878598', piramide:'1-barriga,2- coxa, 3-bumbum, 4-costas', objetivo:'Perder gordura e definir', restricoes:'Nenhuma relatada', academia:'Ct Performance', dataAnamnese:'2025-11-11'},
  {nome:'Tacianne Abreu', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'4x por semana', email:'tacianneabreu@gmail.com', telefone:'51996623475', piramide:'1, 3, 4,2', objetivo:'Definição muscular', restricoes:'Não possuo', academia:'Smart Fit', dataAnamnese:'2025-11-17'},
  {nome:'Rita Lenise de Vargas Ascenço', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'rita.lenise@gmail.com', telefone:'51999165692', piramide:'3 2 4 1', objetivo:'Emagrecer, para saúde e auto estima', restricoes:'Sim', academia:'Posso treinar em casa ou só somente na academia', dataAnamnese:'2025-11-18'},
  {nome:'Lea costa Borges', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'5x por semana', email:'leaborges12@gmail.com', telefone:'51999899593', piramide:'Posterior e glúteos, costas abdômen', objetivo:'Treino mais específico', restricoes:'Sim; Sim; Só no ombro, mas só tenho muito força', academia:'Yes academia', dataAnamnese:'2025-11-18'},
  {nome:'Ester Dalla Valle Lopes', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'A definir', freq:'3x por semana', email:'ester.lopes17@gmail.com', telefone:'51992920228', piramide:'bumbum, coxa, barriga e costas...', objetivo:'Fazer um treinamento assertivo para ter mais qualidade nos treinos, para ter perda de peso, ganho de massa magra, melhorar o condicionamento físico, fortalecer a coluna', restricoes:'tive uma lesão na coluna, não me recordo se era inicio de uma hernia, mas isso foi em 2018, fiz pilates e melhorou muito, como tenho seios muito grandes, sinto muita dor nas costas,; não tenho nenhuma cirurgia que  comprometa os movimentos', academia:'SESC - navegantes', dataAnamnese:'2025-11-19'},
  {nome:'Rafaela Carvalho', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'rafaela.c99@gmail.com', telefone:'51995680554', piramide:'1 glúteo 2 barriga 3 coxa,4- costas', objetivo:'Alavancar meus resultados e chegar com o corpo que desejo', restricoes:'Sinto dores isoladas no joelhos, diante a uma consulta foi diagnosticado desgaste mas até o momento não interferiu nos treinos.', academia:'Ct performance', dataAnamnese:'2025-11-19'},
  {nome:'Íris Cunha', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'4x por semana', email:'iriscunha60@gmail.com', telefone:'923804940', piramide:'3 e 1', objetivo:'Emagrecer e firmar o corpo', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2025-11-20'},
  {nome:'Sandrine', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'sandrine.rosa1@gmail.com', telefone:'51991230734', piramide:'1- bumbum 2-coxas 3-bracos 4-costas', objetivo:'Emagrecimento e definição', restricoes:'Desvio na cervical', academia:'Performance', dataAnamnese:'2025-11-20'},
  {nome:'Carolina Prates', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'carolinaprates99@gmail.com', telefone:'51992101341', piramide:'1º- barriga, 2º perna, 3º bumbum, 4º costas', objetivo:'Emagrecer e definir', restricoes:'Nenhuma relatada', academia:'Pretendo treinar na Performance', dataAnamnese:'2025-11-21'},
  {nome:'Igor Piacini', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'6x por semana', email:'igor_piacini10@hotmail.com', telefone:'51995861984', piramide:'Conjunto completo.', objetivo:'Melhorar a qualidade do treino e consequentemente o fisico', restricoes:'Nenhuma relatada', academia:'Performance fitnnes', dataAnamnese:'2025-11-27'},
  {nome:'schaienne corsini da silva', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'6x por semana', email:'schaicorsini@gmail.com', telefone:'55991967136', piramide:'coxa, bumbum, costas, barriga', objetivo:'fortalecer musculatura, evitar recidiva cancer', restricoes:'estou com um pouco de limitação no braço, devido a cirurgia de retirada de mamas e linfonodos que foi feita em fevereiro deste ano.; somente o braço que sinto um pouco', academia:'26fit ou up nova tramandai', dataAnamnese:'2025-11-28'},
  {nome:'Ândria Rosa Dias', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'5x por semana', email:'andriadias193@gmail.com', telefone:'51 999448920', piramide:'3,4,1,2', objetivo:'Quero secar e definir ao maximo. Não quero aquele corpo monstro, quero o mais slim possivel.', restricoes:'Sim, na perna direita. Quebrei a tibia e a fibia e com isso tenho menos fornça nessa perna e também sinto dores nas costas pq usei muleta por muito tempo.; Sim e tenho Hipertensão Arterial Pulmonar; Na tibia e na fibia', academia:'Varia muito, pois estou sempre viajando', dataAnamnese:'2025-12-01'},
  {nome:'Lizandra da Silva Melos', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'lizanelos@yahoo.com.br', telefone:'51-998274337', piramide:'3- 1- 2 - 4', objetivo:'Ser cobrada para chegar aos resultado.', restricoes:'Coloquei silicone a 2 meses e meio', academia:'Performance', dataAnamnese:'2025-12-02'},
  {nome:'Gabriela', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'A definir', email:'gabrielaamador284@gmail.com', telefone:'51997328538', piramide:'1 bumbum 2 barriga  3 coxa  4 costas', objetivo:'Chegar ao meu desejo não só em estética mais com saúde e bem estar', restricoes:'Sim; Tenho uma hérnia discal ,e um emagioma na coluna e desgaste de algumas vértebras', academia:'26 fitnes', dataAnamnese:'2025-12-08'},
  {nome:'Eduarda Reis', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'3x por semana', email:'eduardareisfreitas00@gmail.com', telefone:'51 999712424', piramide:'1, 2, e 3', objetivo:'Definir o corpo', restricoes:'Sim, no tornozelo', academia:'Smart fit', dataAnamnese:'2025-12-08'},
  {nome:'Gabriel de Matos Teixeira', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'gabmattos489@gmail.com', telefone:'51993890755', piramide:'1° Braços 2° Ombro e costas 3° Peito e Abdômen 4° Pernas', objetivo:'Ganhar massa muscular, definição e conseguir desenvolver força. Tenho também a vontade de aprender melhor a fazer cada exercício e seguir um treino que me faça evoluir de fato.', restricoes:'Nenhuma relatada', academia:'Smart Fit Boubron Wallig', dataAnamnese:'2025-12-09'},
  {nome:'Adriana Schnadelbach', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Avançado', freq:'5x por semana', email:'schnadelbachadriana90@gmail.com', telefone:'51995027512', piramide:'Coxa, bumbum, barriga e costas', objetivo:'Meu objetivo é fazer um treino que me de resultados.', restricoes:'Não tenho lesão, porém me sinto bem fraca nos últimos tempos', academia:'Performance', dataAnamnese:'2025-12-10'},
  {nome:'Amanda de Lima Silveira', status:'lead', statusLabel:'Lead antigo · a confirmar', nivel:'Iniciante', freq:'5x por semana', email:'amandalimasilveira0905@outlook.com', telefone:'51 980476473', piramide:'1 bumbum 2 coxa 3 costas barriga', objetivo:'ter constância nos exercicios fisicos no ano de 2026, estetica', restricoes:'Nenhuma relatada', academia:'yes ou performance', dataAnamnese:'2025-12-30'},
  {nome:'Herica', status:'ok', statusLabel:'Ativa recente', nivel:'A definir', freq:'A definir', email:'herica.pin.lorena@gmail.com', telefone:'51997491112', piramide:'Bumbum,barriga,costas ,pernas', objetivo:'Perder gordura e criar massa muscular', restricoes:'Nenhuma relatada', academia:'Perfomece', dataAnamnese:'2026-01-08'},
  {nome:'Henrique Cogo', status:'ok', statusLabel:'Ativa recente', nivel:'Avançado', freq:'4x por semana', email:'henriquecogo97@gmail.com', telefone:'51998480868', piramide:'1- OMBRO E TRAPEZIO  2- PEITO 3-COXAS 4-BRAÇOS', objetivo:'BUSCAR MELHOR FORMA', restricoes:'NADA', academia:'PERFOMECE', dataAnamnese:'2026-01-08'},
  {nome:'Tainara Arruda', status:'ok', statusLabel:'Ativa recente', nivel:'Iniciante', freq:'6x por semana', email:'tainarafarruda@gmail.com', telefone:'51985406087', piramide:'Barriga, bumbum, coxas, costas', objetivo:'Secar, perder peso, definição', restricoes:'Nenhuma relatada', academia:'Yes fitnes', dataAnamnese:'2026-01-09'},
  {nome:'Jefferson nichetti', status:'ok', statusLabel:'Ativa recente', nivel:'Iniciante', freq:'5x por semana', email:'peckerjefferson@gmail.com', telefone:'51981606548', piramide:'Peito , costas , tríceps e perna', objetivo:'Secar e definir', restricoes:'Desgaste na l3 l4 e l4 l5; Sim', academia:'Yesfitnes', dataAnamnese:'2026-01-09'},
  {nome:'Agatha', status:'ok', statusLabel:'Ativa recente', nivel:'Iniciante', freq:'3x por semana', email:'agathasije@gmail.com', telefone:'51989602709', piramide:'1 barriga  2 coxa  3 bunda  4 costa', objetivo:'Emagrecer e definir', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2026-01-10'},
  {nome:'Rafaela Vargas da Silva', status:'ok', statusLabel:'Ativa recente', nivel:'Iniciante', freq:'5x por semana', email:'vargasdasilvarafaela@gmail.com', telefone:'51 982262405', piramide:'1- barriga, 2- bumbum, 3- coxas, 4- costas', objetivo:'Hipertrofia', restricoes:'Nenhuma relatada', academia:'A do meu condomínio', dataAnamnese:'2026-01-12'},
  {nome:'Josiani', status:'ok', statusLabel:'Ativa recente', nivel:'Iniciante', freq:'5x por semana', email:'josiani.davila@gmail.com', telefone:'51 98121 1879', piramide:'1 bumbum, 2 barriga, 3 coxa, 4 costa', objetivo:'Ter resultado eficientes', restricoes:'Nenhuma relatada', academia:'Sani corpori', dataAnamnese:'2026-01-12'},
  {nome:'Bruna Tierling', status:'ok', statusLabel:'Ativa recente', nivel:'Iniciante', freq:'3x por semana', email:'brunnaltier@gmail.com', telefone:'+52 561 1124600', piramide:'Glúteo, coxas, costas, braços', objetivo:'Fortalecimento muscular', restricoes:'Sim; Condromalasia grau 3, quase 4, com indicação de cirurgia e aplicação de Synvisc one 1x ao ano', academia:'Predio', dataAnamnese:'2026-01-16'},
  {nome:'Katheyn', status:'ok', statusLabel:'Ativa recente', nivel:'Avançado', freq:'5x por semana', email:'katheyncarrero31@gmail.com', telefone:'51984843108', piramide:'1234', objetivo:'Melhorar a qualidade de vida e a estética física', restricoes:'Nenhuma relatada', academia:'Quero um treino q possa executar em casa e parques, mas também em academia', dataAnamnese:'2026-01-17'},
  {nome:'Nathália Pomina da Silva', status:'ok', statusLabel:'Ativa recente', nivel:'A definir', freq:'4x por semana', email:'nathaliapomina@gmail.com', telefone:'51989489995', piramide:'1 bumbum , 2 barriga, 3 coxa, 4 superiores', objetivo:'Ter o corpo que desejo e me sentir bem', restricoes:'Cirurgia no tornozelo mas não compromete em nada', academia:'Usina do corpo', dataAnamnese:'2026-01-19'},
  {nome:'Kryslen Ribeiro', status:'ok', statusLabel:'Ativa recente', nivel:'Avançado', freq:'2x por semana', email:'krys.leen@gmail.com', telefone:'51993829822', piramide:'1º reforço de glúteo e quadríceps 2° costas e ombro 3° abdômen', objetivo:'Fortalecimento para corrida', restricoes:'Sim, no quadril', academia:'Perfect', dataAnamnese:'2026-01-23'},
  {nome:'Francielli Loss Volpatto', status:'ok', statusLabel:'Ativa recente', nivel:'Avançado', freq:'4x por semana', email:'franlvolpatto@gmail.com', telefone:'51 989316231', piramide:'1: Coxas; 2: triceps; 3: glúteo 4: costas', objetivo:'Definição muscular, hipertrofia', restricoes:'Nenhuma relatada', academia:'Engenharia do Corpo', dataAnamnese:'2026-01-26'},
  {nome:'Yasmin', status:'ok', statusLabel:'Ativa recente', nivel:'Iniciante', freq:'5x por semana', email:'yasminccamilo28@gmail.com', telefone:'51996529938', piramide:'1º bumbum 2º coxas 3º barriga 4º costas', objetivo:'alcançar o corpo desejado', restricoes:'lesão ligamentar no tornozelo direito; não tenho', academia:'performance', dataAnamnese:'2026-01-29'},
  {nome:'Luany Braga Prado', status:'ok', statusLabel:'Ativa recente', nivel:'Iniciante', freq:'3x por semana', email:'luanybraga57@gmail.com', telefone:'51980359459', piramide:'1: glúteo 2: coxa 3: costas  4: barriga', objetivo:'Ganhar massa muscular, ficar definida, ganhar mais bumbum e coxas', restricoes:'Nenhuma relatada', academia:'Ct performance', dataAnamnese:'2026-01-29'},
  {nome:'Danielle Kressin', status:'ok', statusLabel:'Ativa recente', nivel:'Avançado', freq:'5x por semana', email:'kressindanny@iclod.com', telefone:'51983140058', piramide:'Coxa/bumbum/costas/abdome', objetivo:'Crescer massa muscular', restricoes:'Já tive lombar', academia:'Moinhos fit', dataAnamnese:'2026-02-03'},
  {nome:'Édlyn Carvalho Schenkel', status:'ok', statusLabel:'Ativa recente', nivel:'Avançado', freq:'5x por semana', email:'edlyncarvalhoschenkel@gmail.com', telefone:'51992266540', piramide:'costas, braços (foco em acabar o tchauzinho e ficar com os braços fortes), barriga, pernas/bumbum', objetivo:'definição, foco específico e dicas (aprender ir até a falha, progredir carga, equilibrar os exercícios corretamente)', restricoes:'nao, canelite as vezes na corrida', academia:'ct performance', dataAnamnese:'2026-02-03'},
  {nome:'Raquel Teixeira', status:'ok', statusLabel:'Ativa recente', nivel:'Iniciante', freq:'5x por semana', email:'raquelsonda79@gmail.com', telefone:'51992841469', piramide:'1 - coxa 2 - braços  3 - bumbum  4- flanco (pneu)', objetivo:'Emagrecer e definir', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2026-02-11'},
  {nome:'Sabine do Nascimento', status:'ok', statusLabel:'Ativa recente', nivel:'Avançado', freq:'3x por semana', email:'ssabinenascimento@gmail.com', telefone:'21979064840', piramide:'1° Abdômen, 2° braços( Tríceps e costas), 3° Glúteos e 4° Pernas (Posterior e Interno de coxas).', objetivo:'Queima de gordura e melhora na composição corporal.', restricoes:'Sim, estou com bursite no quadril esquerdo.; Bursite no quadril esquerdo em tratamento, fisioterapeuta.; Não.', academia:'Smart fit', dataAnamnese:'2026-02-16'},
  {nome:'Kátia Fabiane Schneider', status:'ok', statusLabel:'Ativa recente', nivel:'Avançado', freq:'A definir', email:'katiadj12@hotmail.com', telefone:'51 99207-0519', piramide:'1-bumbum 2-coxa 3-posterior e o 4 deixo a critério do profissional, conforme necessidade.', objetivo:'Deixar o corpo mais harmônico, aumentando os inferiores', restricoes:'coluna lombar - vou enviar o laudo e uma imagem', academia:'Athrix Academia em Feliz e feriados e folgas Mais Fitnnes em Presidente Lucena', dataAnamnese:'2026-02-19'},
  {nome:'Vanessa Morfan', status:'ok', statusLabel:'Ativa recente', nivel:'Iniciante', freq:'5x por semana', email:'vanessamorfan@icloud.com', telefone:'51995836100', piramide:'Barriga - bumbum - coxa e costas', objetivo:'Melhirar o corpo kk e Melhorar a qualidade de vida ênfase na saúde também', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2026-02-20'},
  {nome:'Maria eduarda', status:'ok', statusLabel:'Ativa recente', nivel:'A definir', freq:'2x por semana', email:'eduardacruzermachado2003@gmail.com', telefone:'51986076491', piramide:'barriga,coxa e bunda', objetivo:'ajuda pra melhorar o necessário', restricoes:'escoliose', academia:'perfomance', dataAnamnese:'2026-02-21'},
  {nome:'Sabrina Menger', status:'ok', statusLabel:'Ativa recente', nivel:'Intermediário', freq:'3x por semana', email:'sabrinamenger@gmail.com', telefone:'(51)994962597', piramide:'1 - glúteos 2 - braços 3 - costas 4 - posterior coxa', objetivo:'Definir', restricoes:'Nenhuma relatada', academia:'Moinhos', dataAnamnese:'2026-02-23'},
  {nome:'Cinara Bairros Vida', status:'ok', statusLabel:'Ativa recente', nivel:'Iniciante', freq:'4x por semana', email:'cinarabairros@gmail.com', telefone:'51999592399', piramide:'Barriga, coxa, bumbum e costas', objetivo:'Meu objetivo massa muscular, fortalecimento, definição e melhorar desempenho na corrida.', restricoes:'Não tenho a cartilagem no joelho e tive tendinite pata de ganso devido a corrida.; Somente o cuidado com o joelho.', academia:'Moinhos', dataAnamnese:'2026-02-26'},
  {nome:'Márcia Alves', status:'ok', statusLabel:'Ativa recente', nivel:'Intermediário', freq:'3x por semana', email:'dinerei.marcia@icloud.com', telefone:'51 995475409', piramide:'1 barriga 2 bumbum 3 costas 4 coxa', objetivo:'Emagrecimento', restricoes:'Desgaste de patela do joelho direito e esquerdo - direito está mais comprometido.', academia:'Tons de rosa ou Yess', dataAnamnese:'2026-03-05'},
  {nome:'Simoni Adão Rocha', status:'ok', statusLabel:'Ativa recente', nivel:'Avançado', freq:'5x por semana', email:'simoninutricionista@gmail.com', telefone:'51997974377', piramide:'Glúteo, posterior, abdômen costas', objetivo:'Me ajudar a sair desta estagnação', restricoes:'Um pouco de dores nos joelhos e encaixe da coxa no quadril nos últimos meses', academia:'CT performance e moinhos fitness Bourbon shopping', dataAnamnese:'2026-03-07'},
  {nome:'Renata Brogni da Silva', status:'ok', statusLabel:'Ativa recente', nivel:'Avançado', freq:'A definir', email:'renata_brogni@hotmail.com', telefone:'981818145', piramide:'1-2-4-3', objetivo:'Melhorar a questão do condicionamento físico e flacidez', restricoes:'Tenho tenosinovite no tornozelo direito', academia:'Performace', dataAnamnese:'2026-03-27'},
  {nome:'Betina Gauna Vincenti', status:'ok', statusLabel:'Ativa recente', nivel:'Avançado', freq:'5x por semana', email:'betinagauna504@gmail.com', telefone:'51989601705', piramide:'3- barriga, 1-bumbum, e o resto todo', objetivo:'Emagrecer e ganhar massa moscular', restricoes:'Nenhuma relatada', academia:'CT PERFORMANCE', dataAnamnese:'2026-03-27'},
  {nome:'Eliane Fátima Crupinski', status:'ok', statusLabel:'Ativa recente', nivel:'Intermediário', freq:'3x por semana', email:'elianecrupinskiagro@gmail.com', telefone:'51996504863', piramide:'1- barriga afinar cintura, pochete e peneuzinho. 2- Glúteo 3 perna 4 costas', objetivo:'definir corpo', restricoes:'Nenhuma relatada', academia:'ct performance', dataAnamnese:'2026-03-29'},
  {nome:'JEFFERSON RUBIM FERREIRA', status:'ok', statusLabel:'Ativa recente', nivel:'Avançado', freq:'A definir', email:'rubim_07@hotmail.com', telefone:'51999175222', piramide:'1° Braços 2° Peito 3° Ombro 4° Barriga', objetivo:'Hipertrofia', restricoes:'Joelho esquerdo (mais desconforto na corrida) Motivo: futebol; Apenas desgaste nos joelhos', academia:'Smartfit', dataAnamnese:'2026-04-09'},
  {nome:'Jaíne Menezes dos Santos', status:'ok', statusLabel:'Ativa recente', nivel:'Iniciante', freq:'4x por semana', email:'jaine_menezes13@hotmail.com', telefone:'51997867893', piramide:'Braços Coxa Barriga Bumbum', objetivo:'Saúde e emagrecimento', restricoes:'Fiz cirurgia de Joanete a uns 7 anos atrás.', academia:'Do condomínio', dataAnamnese:'2026-04-09'},
  {nome:'Renata Melo Rodrigues', status:'ok', statusLabel:'Ativa recente', nivel:'Avançado', freq:'4x por semana', email:'renatamelorodrigues1@gmail.com', telefone:'55 51985538500', piramide:'1 abdômen  2 coxas  3 bumbum  4 costas', objetivo:'Emagrecimento sem flacidez', restricoes:'Inflamação no ombro', academia:'Focus gym', dataAnamnese:'2026-04-10'},
  {nome:'Rafael Ferrao', status:'ok', statusLabel:'Ativa recente', nivel:'Iniciante', freq:'3x por semana', email:'rafsferrao@gmail.com', telefone:'51989437623', piramide:'Barriga; Peito; Costas; Coxa', objetivo:'Perder peso', restricoes:'Nenhuma relatada', academia:'Panobianco', dataAnamnese:'2026-04-13'},
  {nome:'Janderson Cunha Roszkowski', status:'ok', statusLabel:'Ativa recente', nivel:'Avançado', freq:'3x por semana', email:'jandogremista@gmail.com', telefone:'51995752625', piramide:'2,3,1,4', objetivo:'Fortalecer e trincar kkkk', restricoes:'Nenhuma relatada', academia:'Vou começar com o seu plano', dataAnamnese:'2026-04-13'},
  {nome:'Franciele Traub', status:'ok', statusLabel:'Ativa recente', nivel:'Iniciante', freq:'5x por semana', email:'traubfranciele@gmail.com', telefone:'51997547979', piramide:'Barriga  Coxa Bumbum Costas', objetivo:'Perder barriga, definir cintura, perna e glúteo kkkkk', restricoes:'Nenhuma relatada', academia:'Ideia fitness', dataAnamnese:'2026-04-17'},
  {nome:'Marcelo dos Reis da silva junior', status:'ok', statusLabel:'Ativa recente', nivel:'A definir', freq:'4x por semana', email:'marceloreissilvajr@gmail.com', telefone:'51996808282', piramide:'Peito ombro Costa barriga', objetivo:'Resultado', restricoes:'Sim tenho uma lesão no joelho direito já fiz cirurgia; No joelho direito', academia:'Smart Fit bourbon Ipiranga e ct performance', dataAnamnese:'2026-04-19'},
  {nome:'Eduarda', status:'ok', statusLabel:'Ativa recente', nivel:'A definir', freq:'4x por semana', email:'duda.mentz@gmail.com', telefone:'51995105977', piramide:'2, 1, 3 e 4', objetivo:'Emagrecer e tonificar', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2026-04-22'},
  {nome:'Gislaine Preto Pires', status:'ok', statusLabel:'Ativa recente', nivel:'Avançado', freq:'2x por semana', email:'gpretopires@gmail.com', telefone:'(51) 99986-7884', piramide:'2', objetivo:'Resultado', restricoes:'Nenhuma relatada', academia:'Performance', dataAnamnese:'2026-04-22'},
  {nome:'Gisele de Abreu Moraes', status:'ok', statusLabel:'Ativa recente', nivel:'A definir', freq:'4x por semana', email:'GiseleMoraes1704@gmail.com', telefone:'51980122191', piramide:'1-coxas 2-glúteo  3- braços (emagreci muito na época que descobri diabetes e fiquei com sobra de pele) 4- costas', objetivo:'Potencializar resultados junto com acompanhamento nutricional, buscando melhorar o processo de emagrecimentos e de hipertrofia', restricoes:'Sim! Rompimento do menisco esquerdo! Cirurgia de meniscectomia realizada em dezembro de 2024 mas hoje vida normal não me limita em nada.; Cirurgia de retirada de menisco', academia:'Prime fit', dataAnamnese:'2026-04-23'},
  {nome:'Viviane da Cunha Moreira', status:'ok', statusLabel:'Ativa recente', nivel:'A definir', freq:'4x por semana', email:'vi.dacunha@gmail.com', telefone:'51 992907258', piramide:'Barriga, coxa, bumbum, costas', objetivo:'Crescer', restricoes:'Não, mas meu joelho está dando sinais da idade, hahahh', academia:'Total', dataAnamnese:'2026-05-04'},
  {nome:'Joice Lima', status:'ok', statusLabel:'Ativa recente', nivel:'Avançado', freq:'5x por semana', email:'joiceclima97@gmail.com', telefone:'51995207714', piramide:'1 quadríceps, bumbum, barriga e costas', objetivo:'Definição', restricoes:'Nenhuma relatada', academia:'Ct performance', dataAnamnese:'2026-05-17'},
  {nome:'Carolina pires', status:'ok', statusLabel:'Ativa recente', nivel:'Avançado', freq:'4x por semana', email:'carolzitz@gmail.com', telefone:'51 999845911', piramide:'Glúteo/ barriga / glúteo / barriga/ glúteo', objetivo:'Emagrecimento e hipertrofia', restricoes:'Nenhuma relatada', academia:'Usina do corpo', dataAnamnese:'2026-05-22'},
  {nome:'Ariel Morais Ferreira', status:'ok', statusLabel:'Ativa recente', nivel:'Avançado', freq:'3x por semana', email:'ariiel.pdc@gmail.com', telefone:'51982362037', piramide:'1 coxa, 2 bumbum, 3 tríceps, 4 barriga', objetivo:'Definir coxa e bumbum', restricoes:'Condromalacia patelar nos dois joelhos e desgaste no menisco no joelho esquedo; Ressonância magnética dos dois joelhos', academia:'Ideia Fitness', dataAnamnese:'2026-06-07'},
  {nome:'Sinara', status:'ok', statusLabel:'Ativa recente', nivel:'Iniciante', freq:'2x por semana', email:'silvasinara704@gmail.com', telefone:'51980165964', piramide:'3 barriga', objetivo:'Perder abdômen', restricoes:'Nenhuma relatada', academia:'Casa', dataAnamnese:'2026-06-11'},
  {nome:'JULIANA HERMES PEREIRA REGO', status:'ok', statusLabel:'Ativa recente', nivel:'A definir', freq:'5x por semana', email:'jhpr16@gmail.com', telefone:'51998005817', piramide:'1- braço / 2 - coxa / 3 - barriga / 4 - costas', objetivo:'Emagrecer e tornear', restricoes:'Nenhuma relatada', academia:'Yes Fitnnes', dataAnamnese:'2026-06-22'},
  {nome:'Andriele Caroline Rubert', status:'ok', statusLabel:'Ativa recente', nivel:'Iniciante', freq:'5x por semana', email:'andrielecarolinerubert@gmail.com', telefone:'51991423839', piramide:'1- bumbum, 2- barriga(perder), 3- pernas, 4- costas', objetivo:'Esse ano decidi que vou focar no meu corpo e deixar ele de um jeito que eu me sinta bem, já perdi muito peso mas nunca fiquei com o corpo que queria, agora estou focada nisso e acredito que tu pode me auxiliar nesse processo', restricoes:'Dor no joelho ao fazer afundo ou stiff, excluídos da prescrição inicial', academia:'Performance', dataAnamnese:'2026-06-30', idade:30, queixaDor:true, regiaoQueixa:'Joelho', desviosPosturaisConfirmados:['ombros_protrusos'],
   peso:'101 kg', altura:'1,69 m', imc:'35,4 (fase 1: emagrecimento prioritário)',
   treinoAtual:{fase:'Fase 1 · Emagrecimento (override por IMC)', volume:'Dentro do limite de iniciante · até 20 séries/semana por ênfase',
     dias:[
       {n:'Segunda', foco:'Inferiores A · Quadríceps e Glúteo', ex:['Leg Press 45 · 4x12','Agachamento Hack · 3x12','Extensão de quadril na máquina · 3x15','Abdução de quadril na polia · 3x15']},
       {n:'Terça', foco:'Superiores · Costas e Abdômen', ex:['Remada articular aberta · 3x12','Remada articular fechada · 3x12','Prancha ventral · 3x30s','Abdominal canivete · 3x15']},
       {n:'Quarta', foco:'Inferiores B · Glúteo e Posterior', ex:['Extensão de quadril no banco romano · 4x15','Cadeira flexora · 3x12','Leg Press 45 (pés altos) · 3x15','Abdução de quadril na polia · 3x15']},
       {n:'Quinta', foco:'Superiores · Costas e Abdômen', ex:['Remada articular aberta · 3x12','Remada articular fechada · 3x12','Prancha ventral · 3x30s','Abdominal canivete · 3x15']},
       {n:'Sexta', foco:'Inferiores C · Pernas completo', ex:['Agachamento Hack · 3x12','Leg Press 45 · 3x15','Cadeira flexora · 3x12','Extensão de quadril na máquina · 3x15']},
       {n:'Sábado', foco:'Descanso', descanso:true, ex:[]},
       {n:'Domingo', foco:'Descanso', descanso:true, ex:[]}
     ]}}
];

// Blindagem: quando a mesma pessoa preencheu a anamnese mais de uma vez ao longo do tempo,
// mantém só a anamnese mais recente, remove as antigas — evita nome duplicado na lista de alunas.
(function removerAlunasDuplicadasPorNome(){
  const maisRecentePorNome = {};
  alunasPersonal.forEach(function(a){
    const chave = a.nome.trim().toUpperCase();
    const dataA = a.dataAnamnese || '0000-00-00';
    if(!maisRecentePorNome[chave] || dataA > maisRecentePorNome[chave].dataAnamnese){
      maisRecentePorNome[chave] = a;
    }
  });
  for(let i = alunasPersonal.length - 1; i >= 0; i--){
    const a = alunasPersonal[i];
    const chave = a.nome.trim().toUpperCase();
    if(maisRecentePorNome[chave] !== a){
      alunasPersonal.splice(i, 1);
    }
  }
})();

let estagioFunilAtivo = 'inicio';
const mensagensExemploFunil = {
  inicio: [
    { titulo: 'Boas-vindas ao time', texto: 'Oi! Que bom ter você com a gente. Nos próximos dias você vai receber seu treino, e qualquer dúvida é só chamar.' },
    { titulo: 'Check-in dos primeiros 15 dias', texto: 'Como estão sendo os primeiros treinos? Alguma dor ou dificuldade em algum exercício que eu deva saber?' }
  ],
  meio: [
    { titulo: 'Acompanhamento de meio de ciclo', texto: 'Vamos revisar como está indo? Me conta se sentiu evolução na carga ou se algo precisa ajustar.' },
    { titulo: 'Manutenção', texto: 'Passando pra saber se está tudo certo com a rotina de treino e se precisa de algum ajuste.' }
  ],
  fim: [
    { titulo: 'Pré-renovação', texto: 'Seu plano está próximo de vencer. Vamos conversar sobre a renovação e já aproveitar pra revisar sua evolução até aqui?' },
    { titulo: 'Carência', texto: 'Seu plano venceu há pouco tempo, ainda dá tempo de renovar sem perder o histórico. Bora continuar?' }
  ]
};

function moverAlunaNoFunil(nomeAluna, novoEstagio){
  if(!novoEstagio) return;
  const a = alunasPersonal.find(function(x){ return x.nome === nomeAluna; });
  if(!a) return;
  a.estagioFunilManual = novoEstagio === 'auto' ? null : novoEstagio;
  salvarPerfilAlunaNoSupabase(nomeAluna);
  alternarEstagioFunil(estagioFunilAtivo);
}

function abrirTelaFunil(){
  showPersonalView('funil');
  alternarEstagioFunil('inicio');
}

function alunaNoEstagioFunil(a, estagio){
  if(a.estagioFunilManual) return a.estagioFunilManual === estagio; // ajuste manual sempre vale mais que o cálculo automático
  let diasNoPrograma, duracaoPlano;
  if(a.dataFechouPlano && a.duracaoPlanoDias){
    const inicio = new Date(a.dataFechouPlano + 'T00:00:00');
    diasNoPrograma = Math.max(0, Math.round((new Date() - inicio) / (1000*60*60*24)));
    duracaoPlano = a.duracaoPlanoDias;
  } else {
    diasNoPrograma = a.diasNoProgramaFunil == null ? 1 : a.diasNoProgramaFunil;
    duracaoPlano = DURACAO_PLANO_DIAS;
  }
  const fase = calcularFaseFunil(diasNoPrograma, duracaoPlano).fase;
  if(estagio === 'inicio') return fase === 'Boas-vindas';
  if(estagio === 'meio') return fase === 'Ritmo reduzido' || fase === 'Pré-renovação';
  return fase === 'Carência' || fase === 'Carência expirada';
}

function alternarEstagioFunil(estagio){
  estagioFunilAtivo = estagio;
  ['inicio','meio','fim'].forEach(function(e){
    const el = document.getElementById('funil-tab-' + e);
    if(el) el.classList.toggle('active', e === estagio);
  });

  const alunasNoEstagio = alunasPersonal.filter(function(a){ return alunaNoEstagioFunil(a, estagio); });
  const lista = document.getElementById('funil-lista-alunas');
  lista.innerHTML = alunasNoEstagio.length === 0
    ? '<div class="info-box"><p class="txt">Nenhuma aluna nesse estágio agora.</p></div>'
    : alunasNoEstagio.map(function(a){
        const i = alunasPersonal.indexOf(a);
        return '<div class="aluna-card" style="flex-direction:column;align-items:stretch;gap:6px;">' +
          '<div style="display:flex;justify-content:space-between;align-items:center;cursor:pointer;" onclick="openAlunaDetail(' + i + ')">' +
            '<div><p class="aluna-name">' + a.nome + '</p><p class="aluna-meta">' + a.nivel + (a.estagioFunilManual ? ' · movida manualmente' : '') + '</p></div>' +
            '<i class="ti ti-chevron-right" style="color:var(--text-faint);"></i>' +
          '</div>' +
          '<select class="form-select" style="font-size:11px;padding:4px 6px;" onchange="moverAlunaNoFunil(\'' + a.nome.replace(/'/g,"\\'") + '\',this.value)">' +
            '<option value="">Mover pra outro estágio...</option>' +
            '<option value="inicio"' + (estagio==='inicio'?' disabled':'') + '>Início</option>' +
            '<option value="meio"' + (estagio==='meio'?' disabled':'') + '>Meio</option>' +
            '<option value="fim"' + (estagio==='fim'?' disabled':'') + '>Fim</option>' +
            (a.estagioFunilManual ? '<option value="auto">Voltar ao automático</option>' : '') +
          '</select>' +
        '</div>';
      }).join('');

  const notif = document.getElementById('funil-notificacoes');
  notif.innerHTML = mensagensExemploFunil[estagio].map(function(m){
    return '<div class="info-box"><p class="lbl">' + m.titulo + '</p><p class="txt" style="margin-bottom:0;">' + m.texto + '</p></div>';
  }).join('');
}

let filtroAlunasAtivo = 'ativas';

function alternarFiltroAlunas(filtro){
  filtroAlunasAtivo = filtro;
  ['ativas', 'porvencer', 'vencidas'].forEach(function(f){
    const el = document.getElementById('filtro-tab-' + f);
    if(el) el.classList.toggle('active', f === filtro);
  });
  renderAlunas();
}

function statusDoPlano(a){
  if(a.statusPlanoManual) return a.statusPlanoManual; // ajuste manual sempre vale mais que o cálculo automático
  if(!a.dataFechouPlano || !a.duracaoPlanoDias) return 'ativas'; // sem plano registrado ainda, considera ativa por padrão
  const dataFim = new Date(a.dataFechouPlano);
  dataFim.setDate(dataFim.getDate() + parseInt(a.duracaoPlanoDias, 10));
  const hoje = new Date();
  const diasRestantes = Math.floor((dataFim - hoje) / (1000*60*60*24));
  if(diasRestantes < 0) return 'vencidas';
  if(diasRestantes <= 15) return 'porvencer';
  return 'ativas';
}

function moverAlunaDeStatus(nomeAluna, novoStatus){
  if(!novoStatus) return;
  const a = alunasPersonal.find(function(x){ return x.nome === nomeAluna; });
  if(!a) return;
  a.statusPlanoManual = novoStatus === 'auto' ? null : novoStatus;
  salvarPerfilAlunaNoSupabase(nomeAluna);
  renderAlunas();
}

// ===== SELEÇÃO MÚLTIPLA DE ALUNAS (marcar várias e mover todas de uma vez) =====
let modoSelecaoAlunasAtivo = false;
let alunasSelecionadas = new Set();

function alternarModoSelecaoAlunas(){
  modoSelecaoAlunasAtivo = !modoSelecaoAlunasAtivo;
  alunasSelecionadas.clear();
  const chip = document.getElementById('chip-modo-selecao');
  if(chip) chip.classList.toggle('active', modoSelecaoAlunasAtivo);
  const barra = document.getElementById('barra-selecao-alunas');
  if(barra) barra.style.display = modoSelecaoAlunasAtivo ? 'flex' : 'none';
  renderAlunas();
}

function alternarSelecaoAluna(nome){
  if(alunasSelecionadas.has(nome)) alunasSelecionadas.delete(nome); else alunasSelecionadas.add(nome);
  const contagem = document.getElementById('contagem-selecao-alunas');
  if(contagem) contagem.textContent = alunasSelecionadas.size + ' selecionada(s)';
  renderAlunas();
}

function confirmarMoverAlunasSelecionadas(){
  if(alunasSelecionadas.size === 0){ alert('Marca pelo menos uma aluna antes de mover.'); return; }
  const destino = document.getElementById('destino-mover-alunas').value;
  const rotulos = { ativas: 'Ativas', porvencer: 'Por vencer', vencidas: 'Vencidas' };
  if(!confirm('Mover ' + alunasSelecionadas.size + ' aluna(s) pra "' + rotulos[destino] + '"?')) return;
  alunasSelecionadas.forEach(function(nome){
    const a = alunasPersonal.find(function(x){ return x.nome === nome; });
    if(a){ a.statusPlanoManual = destino; salvarPerfilAlunaNoSupabase(nome); }
  });
  alternarModoSelecaoAlunas(); // já limpa a seleção, esconde a barra e re-renderiza
}

function ehAlunaAntiga(a){
  if(!a.dataAnamnese) return false;
  const ano = parseInt(String(a.dataAnamnese).slice(0, 4), 10);
  return !isNaN(ano) && ano <= 2024;
}

let mostrarSoAlunasAntigas = false;

function abrirAlunasAntigas(){
  openLevel2('personal');
  showPersonalView('alunas');
  mostrarSoAlunasAntigas = true; // define DEPOIS de showPersonalView, que já reseta pra false por padrão
  renderAlunas();
}

function voltarParaTodasAlunas(){
  mostrarSoAlunasAntigas = false;
  renderAlunas();
}

function renderAlunas(){
  const list = document.getElementById('alunas-list');
  const termo = (document.getElementById('aluna-search') ? document.getElementById('aluna-search').value : '').toUpperCase();
  list.innerHTML = '';
  renderAlertasRiscoEmocional();

  const avisoAntigas = document.getElementById('aviso-modo-antigas');
  if(avisoAntigas){
    avisoAntigas.style.display = mostrarSoAlunasAntigas ? 'flex' : 'none';
  }

  const filtradas = alunasPersonal.filter(function(a){
    const bateBusca = !termo || a.nome.toUpperCase().indexOf(termo) !== -1;
    const bateStatus = statusDoPlano(a) === filtroAlunasAtivo;
    const bateAntiguidade = mostrarSoAlunasAntigas ? ehAlunaAntiga(a) : true;
    return bateBusca && bateStatus && bateAntiguidade;
  }).sort(function(a, b){ return a.nome.localeCompare(b.nome, 'pt-BR'); });
  document.getElementById('alunas-count-label').textContent = filtradas.length + ' de ' + alunasPersonal.length + ' alunas';
  if(!window.alunasQtdVisivel) window.alunasQtdVisivel = 80;
  filtradas.slice(0, window.alunasQtdVisivel).forEach(function(a){
    const i = alunasPersonal.indexOf(a);
    const el = document.createElement('div');
    el.className = 'aluna-card';
    el.style.cssText = 'flex-direction:column;align-items:stretch;gap:6px;';
    const financeiro = statusFinanceiroAluna(a);
    const marcada = alunasSelecionadas.has(a.nome);
    el.innerHTML =
      '<div style="display:flex;justify-content:space-between;align-items:center;cursor:pointer;gap:8px;" data-abrir="1">' +
        (modoSelecaoAlunasAtivo ? '<input type="checkbox" class="checkbox-selecao-aluna" ' + (marcada ? 'checked' : '') + ' style="flex-shrink:0;width:18px;height:18px;">' : '') +
        '<div style="flex:1;"><p class="aluna-name">' + a.nome + (a.recemChegadaDaAnamnese ? ' <span class="tag" style="background:var(--success-soft);color:var(--success);">nova</span>' : '') + (!ehAlunaAntiga(a) && a.dataAnamnese ? ' <span class="tag" style="background:var(--gold-soft);color:#1A1409;">2025</span>' : '') + '</p><p class="aluna-meta">' + a.nivel + ' · ' + a.freq + (a.statusPlanoManual ? ' · movida manualmente' : '') + (a.valorPlano != null ? ' · R$ ' + a.valorPlano.toFixed(2).replace('.', ',') : '') + '</p></div>' +
        '<span style="display:flex;align-items:center;gap:6px;">' +
          '<span class="tag" style="background:' + financeiro.corSuave + ';color:' + financeiro.cor + ';">' + financeiro.label + '</span>' +
          '<span class="status-dot ' + a.status + '"></span><span class="status-txt ' + a.status + '">' + a.statusLabel + '</span>' +
        '</span>' +
      '</div>';
    if(modoSelecaoAlunasAtivo){
      el.querySelector('[data-abrir]').onclick = function(){ alternarSelecaoAluna(a.nome); };
    } else {
      el.querySelector('[data-abrir]').onclick = function(){ openAlunaDetail(i); };
    }

    if(!modoSelecaoAlunasAtivo){
      const seletor = document.createElement('select');
      seletor.className = 'form-select';
      seletor.style.cssText = 'font-size:11px;padding:4px 6px;';
      seletor.innerHTML = '<option value="">Mover pra outra aba...</option>' +
        ['ativas','porvencer','vencidas'].filter(function(s){ return s !== filtroAlunasAtivo; }).map(function(s){
          const rotulos = { ativas: 'Ativas', porvencer: 'Por vencer', vencidas: 'Vencidas' };
          return '<option value="' + s + '">' + rotulos[s] + '</option>';
        }).join('') +
        (a.statusPlanoManual ? '<option value="auto">Voltar ao automático</option>' : '');
      seletor.onchange = function(){ moverAlunaDeStatus(a.nome, seletor.value); };
      el.appendChild(seletor);
    }

    list.appendChild(el);
  });
  if(filtradas.length > window.alunasQtdVisivel){
    const btn = document.createElement('button');
    btn.className = 'btn-gold';
    btn.style.cssText = 'background:var(--card-2);color:var(--gold-soft);border:1px solid var(--border);margin-top:8px;';
    btn.textContent = 'Carregar mais (' + (filtradas.length - window.alunasQtdVisivel) + ' restantes)';
    btn.onclick = function(){ window.alunasQtdVisivel += 80; renderAlunas(); };
    list.appendChild(btn);
  }
  if(filtradas.length > 80){
    const note = document.createElement('p');
    note.className = 'page-sub';
    note.textContent = 'Mostrando 80 de ' + filtradas.length + ', refine a busca para ver outras.';
    list.appendChild(note);
  }
}
renderAlunas();

const desviosPosturaisCatalogo = [
  { id: 'cabeca_anteriorizada', nome: 'Anteriorização de cabeça', mobilidade: ['Alongamento de cervical posterior', 'Mobilidade de coluna torácica'], corretivo: ['Retração cervical (chin tuck)', 'Fortalecimento de flexores profundos do pescoço'] },
  { id: 'ombros_protrusos', nome: 'Ombros protrusos / Abdução escapular', mobilidade: ['Alongamento de peitoral maior e menor', 'Alongamento de grande dorsal', 'Alongamento de redondo maior'], corretivo: ['Remada com retração escapular', 'Face pull'] },
  { id: 'hipercifose', nome: 'Hipercifose torácica', mobilidade: ['Mobilidade de coluna torácica em extensão'], corretivo: ['Remada alta', 'Extensão torácica no rolo'] },
  { id: 'escoliose_leve', nome: 'Escoliose leve (não estrutural)', mobilidade: ['Alongamento lateral de tronco'], corretivo: ['Prancha lateral', 'Exercício unilateral assimétrico conforme lado fraco'] },
  { id: 'hiperlordose', nome: 'Hiperlordose lombar (síndrome cruzada inferior)', mobilidade: ['Alongamento de iliopsoas', 'Alongamento de paravertebrais lombares', 'Alongamento de reto femoral', 'Alongamento de tensor da fáscia lata'], corretivo: ['Fortalecimento de glúteo', 'Fortalecimento de isquiotibiais', 'Fortalecimento de reto abdominal e transverso abdominal', 'Prancha ventral', 'Elevação pélvica com retroversão ativa'] },
  { id: 'retroversao_pelvica', nome: 'Retroversão pélvica', mobilidade: ['Alongamento de posterior de coxa', 'Alongamento de glúteo'], corretivo: ['Ativação de flexor de quadril', 'Fortalecimento de eretores lombares'] },
  { id: 'anteversao_pelvica', nome: 'Anteversão pélvica', mobilidade: ['Alongamento de flexor de quadril', 'Alongamento de lombar'], corretivo: ['Prancha ventral', 'Fortalecimento de abdômen inferior'] },
  { id: 'assimetria_quadril', nome: 'Assimetria de quadril / Desnivelamento pélvico (funcional)', mobilidade: ['Alongamento de quadrado lombar do lado mais alto', 'Alongamento de adutor do lado mais alto'], corretivo: ['Fortalecimento de glúteo médio bilateral', 'Exercício unilateral no lado mais fraco com volume extra', 'Ponte unilateral'] },
  { id: 'geno_valgo', nome: 'Geno valgo (joelho para dentro)', mobilidade: ['Mobilidade de tornozelo'], corretivo: ['Fortalecimento de glúteo médio', 'Abdução de quadril na polia'] },
  { id: 'geno_varo', nome: 'Geno varo (joelho para fora)', mobilidade: ['Alongamento de trato iliotibial'], corretivo: ['Fortalecimento de adutores', 'Controle motor de alinhamento de joelho'] },
  { id: 'pe_pronado', nome: 'Pé pronado', mobilidade: ['Mobilidade de tornozelo em inversão'], corretivo: ['Fortalecimento de tibial posterior', 'Propriocepção em superfície instável'] },
  { id: 'pe_supinado', nome: 'Pé supinado', mobilidade: ['Mobilidade de tornozelo em eversão'], corretivo: ['Fortalecimento de fibulares', 'Propriocepção em superfície instável'] },
  { id: 'sway_back', nome: 'Sway Back', mobilidade: ['Alongamento de peitoral e grande dorsal', 'Alongamento de reto femoral'], corretivo: ['Fortalecimento de isquiotibiais', 'Retração escapular (rombóides/trapézio)', 'Fortalecimento de glúteo máximo', 'Prancha ventral com retroversão ativa'] },
  { id: 'escapula_alada', nome: 'Escápula alada', mobilidade: ['Mobilidade torácica'], corretivo: ['Wall slide', 'Wall slide com elástico', 'Push-up plus', 'Fortalecimento de serrátil anterior e rombóides'] },
  { id: 'aducao_escapular', nome: 'Adução escapular (tônus alto de trapézio/rombóides)', mobilidade: ['Mobilidade torácica', 'Alongamento de trapézio e rombóides'], corretivo: ['Push-up plus', 'Wall ball slide', 'Fortalecimento de peitoral', 'Fortalecimento de serrátil anterior'] },
  { id: 'elevacao_escapular', nome: 'Elevação escapular', mobilidade: ['Alongamento de trapézio superior', 'Alongamento de levantador da escápula'], corretivo: ['Wall ball slide', 'Wall slide com elástico', 'Elevação em Y', 'Depressão escapular ativa (evitar puxada full overhead)'] },
  { id: 'deslizamento_anterior_umero', nome: 'Deslizamento anterior do úmero', mobilidade: ['Alongamento de peitoral maior e menor', 'Alongamento de grande dorsal', 'Alongamento de redondo maior', 'Mobilidade de cápsula posterior do ombro'], corretivo: ['Fortalecimento de deltoide posterior', 'Fortalecimento no plano sagital', 'Atenção à amplitude completa na fase concêntrica'] },
  { id: 'rotacao_inferior_escapular', nome: 'Rotação inferior escapular (= Depressão escapular)', mobilidade: ['Alongamento de grande dorsal e peitoral maior/menor'], corretivo: ['Fortalecimento de trapézio superior', 'Elevação em Y', 'Encolhimento de ombros'] },
  { id: 'retificacao_toracica', nome: 'Retificação torácica', mobilidade: ['Alongamento de peitoral maior e menor', 'Alongamento de grande dorsal'], corretivo: ['Remada articulada', 'Remada baixa', 'Remada no cross (3x12-15, 1min descanso, fase concêntrica completa e excêntrica controlada)'] },
  { id: 'retificacao_lombar', nome: 'Retificação lombar', mobilidade: ['Alongamento de isquiotibiais e adutores', 'Alongamento de panturrilha', 'Alongamento de oblíquo interno e reto abdominal'], corretivo: ['Fortalecimento de iliopsoas e paravertebrais lombares', 'Stiff', 'Remada curvada', 'Superman no solo', 'Ponte unilateral/bilateral'] }
];

function obterBlocoPostural(desviosConfirmados){
  if(!desviosConfirmados || desviosConfirmados.length === 0) return [];
  const exercicios = [];
  desviosConfirmados.forEach(function(id){
    const desvio = desviosPosturaisCatalogo.find(function(d){ return d.id === id; });
    if(!desvio) return;
    if(desvio.mobilidade[0]) exercicios.push({ nome: desvio.mobilidade[0], tipo: 'mobilidade' });
    if(desvio.corretivo[0]) exercicios.push({ nome: desvio.corretivo[0], tipo: 'corretivo' });
  });
  return exercicios.slice(0, 3).map(function(item){
    if(item.tipo === 'mobilidade'){
      return { nome: item.nome, volume: '2x30s', descanso: '20s' };
    }
    return { nome: item.nome, volume: '3x15', descanso: '30-40s' };
  });
}

const patologiasCatalogo = [
  {id:'condromalacia', regiao:'Joelho', nome:'Condromalácia patelar (graus 1-4)', oQueE:'Amolecimento/desgaste da cartilagem atrás da patela, com dor ao agachar, subir/descer escada ou ficar muito tempo sentada.', evitar:'Agachamento profundo com carga alta; cadeira extensora em amplitude completa com carga alta; saltos quando sintomática.', permitido:'Leg press com carga baixa e agachamento parcial; isométricos de quadríceps; elevação pélvica e ponte unilateral; baixo impacto (bike).', conduta:'Fortalecer glúteo médio e rotadores externos para controlar o valgo dinâmico; priorizar cadeia fechada com amplitude reduzida antes de progredir.'},
  {id:'menisco', regiao:'Joelho', nome:'Lesão de menisco', oQueE:'Lesão na cartilagem em formato de C que amortece o joelho, comum em movimentos de torção com o pé fixo no chão.', evitar:'Agachamento com rotação; avanço/afundo com pivô; leg press com pés mal posicionados gerando torção.', permitido:'Leg press com trajetória controlada; extensão/flexão de joelho sem rotação; fortalecimento isométrico.', conduta:'Controlar amplitude, evitar flexão profunda combinada com rotação; progressão de carga mais lenta que o padrão.'},
  {id:'artrose_joelho', regiao:'Joelho', nome:'Osteoartrose de joelho (desgaste articular)', oQueE:'Desgaste progressivo da cartilagem articular, mais comum com histórico de treino longo ou idade avançada.', evitar:'Impacto repetitivo (corrida, salto); amplitude completa com carga alta em fases sintomáticas.', permitido:'Fortalecimento muscular ao redor da articulação; exercícios de baixo impacto.', conduta:'Fortalecer sem aumentar o atrito articular, cadeia fechada controlada, evitar picos de carga.'},
  {id:'tendinopatia_patelar', regiao:'Joelho', nome:'Tendinopatia patelar ("joelho de saltador")', oQueE:'Inflamação/degeneração do tendão patelar, geralmente por sobrecarga.', evitar:'Alto volume de saltos ou agachamentos explosivos enquanto sintomática.', permitido:'Trabalho excêntrico controlado; cargas moderadas e progressivas.', conduta:'Gestão de carga é mais eficaz que repouso total, reduzir volume, não necessariamente parar.'},
  {id:'fai', regiao:'Quadril', nome:'Impacto femoroacetabular (FAI) / lesão labral do quadril', oQueE:'Atrito anormal entre o fêmur e o encaixe do quadril, ou lesão na cartilagem que reveste esse encaixe.', evitar:'Agachamento muito profundo; flexão de quadril extrema sob carga.', permitido:'Agachamento com amplitude controlada; exercícios de estabilização de quadril.', conduta:'Ajustar amplitude antes de reduzir carga; controlar rotação do quadril durante o movimento.'},
  {id:'bursite_trocanterica', regiao:'Quadril', nome:'Bursite trocantérica', oQueE:'Inflamação da bolsa sinovial na lateral do quadril, causa dor lateral em quem faz muita abdução repetitiva.', evitar:'Volume alto de abdução de quadril unilateral repetitiva; deitar sobre o lado afetado com pressão direta.', permitido:'Fortalecimento de glúteo médio com volume controlado; exercícios bilaterais.', conduta:'Reduzir volume de abdução isolada temporariamente, sem eliminar o estímulo por completo.'},
  {id:'coxartrose', regiao:'Quadril', nome:'Osteoartrose de quadril (coxartrose)', oQueE:'Desgaste articular do quadril, similar ao de joelho.', evitar:'Amplitude extrema combinada com carga alta.', permitido:'Fortalecimento progressivo; mobilidade controlada.', conduta:'Fortalecer para proteger a articulação, sem forçar amplitude dolorosa.'},
  {id:'tendinopatia_glutea', regiao:'Quadril', nome:'Tendinopatia glútea', oQueE:'Sobrecarga do tendão do glúteo médio/mínimo na lateral do quadril, comum em mulheres.', evitar:'Compressão excessiva do tendão (adução extrema, deitar de lado sem apoio); volume alto de abdução.', permitido:'Fortalecimento progressivo evitando posições de compressão extrema.', conduta:'Cuidado especial com abdução em decúbito lateral, ajustar ângulo.'},
  {id:'impacto_ombro', regiao:'Ombro', nome:'Síndrome do impacto / tendinopatia do manguito rotador', oQueE:'Compressão dos tendões do manguito rotador no espaço subacromial, dor ao elevar o braço acima da cabeça.', evitar:'Desenvolvimento por trás da nuca; elevação lateral acima da linha do ombro com carga alta; supino pesado com cotovelos muito abertos.', permitido:'Elevações controladas até a linha dos ombros; fortalecimento de manguito com carga leve.', conduta:'Fortalecer estabilizadores da escápula antes de progredir carga em empurrar/elevar.'},
  {id:'labral_ombro', regiao:'Ombro', nome:'Lesão labral do ombro (SLAP)', oQueE:'Lesão na cartilagem que estabiliza a cabeça do úmero na cavidade do ombro.', evitar:'Movimentos overhead com carga alta; puxadas por trás da nuca.', permitido:'Exercícios de estabilização escapular; amplitude controlada frontal.', conduta:'Priorizar controle motor e estabilidade antes de carga.'},
  {id:'instabilidade_ombro', regiao:'Ombro', nome:'Instabilidade / luxação recidivante de ombro', oQueE:'Frouxidão articular que permite deslocamentos parciais ou completos do ombro.', evitar:'Amplitude extrema em rotação externa com carga (ex: crucifixo muito aberto).', permitido:'Fortalecimento de manguito rotador e estabilizadores; amplitude controlada.', conduta:'Sempre priorizar controle sobre amplitude máxima.'},
  {id:'bursite_subacromial', regiao:'Ombro', nome:'Bursite subacromial', oQueE:'Inflamação da bolsa sinovial sob o acrômio, geralmente associada à síndrome do impacto.', evitar:'Elevação repetitiva acima da linha do ombro.', permitido:'Exercícios abaixo da linha do ombro; fortalecimento de estabilizadores.', conduta:'Reduzir compressão, fortalecer ao redor.'}
];

let filtroPatologia = 'Todas';

function renderPatologiaChips(){
  const row = document.getElementById('patologia-filter-row');
  row.innerHTML = '';
  ['Todas','Joelho','Quadril','Ombro'].forEach(function(cat){
    const chip = document.createElement('div');
    chip.className = 'chip' + (cat === filtroPatologia ? ' active' : '');
    chip.textContent = cat;
    chip.onclick = function(){ filtroPatologia = cat; renderPatologiaChips(); renderPatologiaList(); };
    row.appendChild(chip);
  });
}

function renderPatologiaList(){
  const list = document.getElementById('patologia-list');
  list.innerHTML = '';
  const itens = filtroPatologia === 'Todas' ? patologiasCatalogo : patologiasCatalogo.filter(function(p){ return p.regiao === filtroPatologia; });
  itens.forEach(function(p){
    const el = document.createElement('div');
    el.className = 'info-box';
    el.innerHTML =
      '<p class="lbl">' + p.regiao + '</p>' +
      '<p class="card-item-title">' + p.nome + '</p>' +
      '<p class="txt"><b>O que é:</b> ' + p.oQueE + '</p>' +
      '<p class="txt"><b>Evitar:</b> ' + p.evitar + '</p>' +
      '<p class="txt"><b>Permitido:</b> ' + p.permitido + '</p>' +
      '<p class="txt" style="margin-bottom:0;"><b>Conduta:</b> ' + p.conduta + '</p>';
    list.appendChild(el);
  });
}

const testesCorridaCatalogo = [
  { nome: 'Teste de Cooper (12 minutos)', tipo: 'Teste', desc: 'Correr a maior distância possível em 12 minutos. Estima o VO2máx e serve como linha de base pra prescrever intensidade dos treinos.' },
  { nome: 'Teste de 1km contra o relógio', tipo: 'Teste', desc: 'Correr 1km no menor tempo possível. Usado pra estimar o ritmo de limiar e calibrar as zonas de treino.' },
  { nome: 'Teste de limiar (Conconi indireto)', tipo: 'Teste', desc: 'Corrida progressiva em esteira ou pista, aumentando a velocidade a cada estágio, observando onde a frequência cardíaca deixa de subir proporcionalmente.' },
  { nome: 'Rodagem contínua (base aeróbica)', tipo: 'Treino', desc: 'Corrida em ritmo confortável, constante, por 30-60min. Constrói a base aeróbica, entra na maioria das semanas.' },
  { nome: 'Treino intervalado', tipo: 'Treino', desc: 'Blocos de corrida forte alternados com recuperação (ex: 8x400m forte + 200m trote). Melhora velocidade e capacidade anaeróbica.' },
  { nome: 'Fartlek', tipo: 'Treino', desc: 'Variação livre de ritmo durante a corrida, sem intervalos cronometrados rígidos, alterna forte e leve conforme a sensação.' },
  { nome: 'Tiro / Sprint', tipo: 'Treino', desc: 'Corridas curtas e muito intensas (50-200m) com recuperação completa entre elas. Foco em potência e velocidade máxima.' },
  { nome: 'Rodagem longa', tipo: 'Treino', desc: 'Corrida mais longa que o normal, em ritmo confortável, geralmente 1x por semana. Constrói resistência de base pra provas mais longas.' },
  { nome: 'Corrida regenerativa', tipo: 'Treino', desc: 'Corrida bem leve, curta, no dia seguinte a um treino intenso, ajuda na recuperação ativa sem acumular fadiga extra.' }
];

function renderCorridaBanco(){
  const list = document.getElementById('corrida-list');
  let html = '';
  ['Teste', 'Treino'].forEach(function(secao){
    html += '<p class="section-label" style="margin-top:16px;">' + (secao === 'Teste' ? 'Testes de avaliação' : 'Tipos de treino') + '</p>';
    testesCorridaCatalogo.filter(function(t){ return t.tipo === secao; }).forEach(function(t){
      html += '<div class="info-box"><p class="card-item-title">' + t.nome + '</p><p class="txt" style="margin-bottom:0;">' + t.desc + '</p></div>';
    });
  });
  list.innerHTML = html;
}

function renderDesviosBanco(){
  const list = document.getElementById('desvios-list');
  let html = '';
  desviosPosturaisCatalogo.forEach(function(d){
    html += '<div class="info-box">' +
      '<p class="card-item-title">' + d.nome + '</p>' +
      '<p class="txt"><b>Mobilidade:</b> ' + d.mobilidade.join(', ') + '</p>' +
      '<p class="txt" style="margin-bottom:0;"><b>Corretivo:</b> ' + d.corretivo.join(', ') + '</p>' +
      '</div>';
  });
  list.innerHTML = html;
}

function confirmarPatologia(nomeAluna, patologiaId){
  const a = alunasPersonal.find(function(x){ return x.nome === nomeAluna; });
  if(!a) return;
  a.patologiaConfirmada = patologiaId === 'tecnica' ? 'tecnica' : patologiaId;
  salvarPerfilAlunaNoSupabase(nomeAluna);
  const i = alunasPersonal.indexOf(a);
  openAlunaDetail(i);
}

const progressoesPorAluna = {};
let alunaAberta = null;
let detailDiaAtual = null;

function getProgressoAluna(nome){
  if(!progressoesPorAluna[nome]){
    progressoesPorAluna[nome] = { semana: 1, historico: {}, diasConcluidos: {}, substituicoes: [], nutricao: {} };
  }
  return progressoesPorAluna[nome];
}

function detectarPadraoTreino(nome){
  const prog = getProgressoAluna(nome);
  const historico = prog.horariosTreino || [];
  if(historico.length < 4){
    return { temPadrao: false };
  }

  const contagemDias = {};
  let somaMinutos = 0;
  historico.forEach(function(h){
    contagemDias[h.dia] = (contagemDias[h.dia] || 0) + 1;
    somaMinutos += (h.hora * 60 + h.minuto);
  });

  const diasOrdenados = Object.keys(contagemDias).sort(function(a, b){ return contagemDias[b] - contagemDias[a]; });
  const diasTipicos = diasOrdenados.slice(0, 3);

  const mediaMinutos = Math.round(somaMinutos / historico.length);
  const horaTypica = Math.floor(mediaMinutos / 60);
  const minutoTypico = mediaMinutos % 60;
  const periodoDia = horaTypica < 12 ? 'manhã' : (horaTypica < 18 ? 'tarde' : 'noite');

  return {
    temPadrao: true,
    diasTipicos: diasTipicos,
    horario: String(horaTypica).padStart(2,'0') + ':' + String(minutoTypico).padStart(2,'0'),
    periodoDia: periodoDia,
    totalRegistros: historico.length
  };
}

function decidirDiaDeCheckIn(nome){
  const hoje = new Date();
  const diaDoAno = Math.floor((hoje - new Date(hoje.getFullYear(), 0, 0)) / (1000*60*60*24));
  let hash = 0;
  for(let i = 0; i < nome.length; i++) hash = (hash * 31 + nome.charCodeAt(i)) % 1000;
  return ((diaDoAno + hash) % 7) < 3;
}

function registrarRespostaCheckIn(nome, vaiTreinar){
  const prog = getProgressoAluna(nome);
  if(!prog.respostasCheckIn) prog.respostasCheckIn = [];
  prog.respostasCheckIn.push({ vaiTreinar: vaiTreinar, data: new Date().toISOString() });
  salvarProgressoNoSupabase(nome);
  return prog.respostasCheckIn;
}

function calcularSequenciaCheckIn(respostas){
  let seq = 0;
  for(let i = respostas.length - 1; i >= 0; i--){
    if(respostas[i].vaiTreinar === respostas[respostas.length - 1].vaiTreinar) seq++;
    else break;
  }
  return seq;
}

// ===== CHECK-IN EMOCIONAL DIÁRIO + HÁBITOS COM PONTUAÇÃO =====
const CATALOGO_HABITOS_DIARIOS = [
  { id: 'treino', label: 'Treino concluído', icone: 'ti-barbell', pontos: 10 },
  { id: 'hidratacao', label: 'Hidratação em dia', icone: 'ti-droplet', pontos: 5 },
  { id: 'alimentacao', label: 'Alimentação registrada', icone: 'ti-salad', pontos: 5 },
  { id: 'sono', label: 'Sono de qualidade (7h+)', icone: 'ti-moon', pontos: 5 },
  { id: 'autocuidado', label: 'Momento de autocuidado', icone: 'ti-flower', pontos: 5 },
  { id: 'meditou', label: 'Meditou', icone: 'ti-yoga', pontos: 10 }
];

function getDataHojeISO(){
  return new Date().toISOString().slice(0,10);
}

function getCheckInHoje(nome){
  const prog = getProgressoAluna(nome);
  if(!prog.checkinsEmocionais) prog.checkinsEmocionais = {};
  const hoje = getDataHojeISO();
  if(!prog.checkinsEmocionais[hoje]) prog.checkinsEmocionais[hoje] = { data: hoje };
  return prog.checkinsEmocionais[hoje];
}

function salvarCampoCheckInEmocional(nome, campo, valor){
  const checkin = getCheckInHoje(nome);
  checkin[campo] = valor;
  salvarProgressoNoSupabase(nome);
  renderCheckInEHabitos(nome);
}

function getHabitosHojeAluna(nome){
  const prog = getProgressoAluna(nome);
  if(!prog.habitosDoDia) prog.habitosDoDia = {};
  const hoje = getDataHojeISO();
  if(!prog.habitosDoDia[hoje]) prog.habitosDoDia[hoje] = {};
  return prog.habitosDoDia[hoje];
}

function alternarHabitoDoDia(nome, habitoId){
  const habitosHoje = getHabitosHojeAluna(nome);
  habitosHoje[habitoId] = !habitosHoje[habitoId];
  salvarProgressoNoSupabase(nome);
  renderCheckInEHabitos(nome);
}

function calcularPontosHabitosHoje(nome){
  const habitosHoje = getHabitosHojeAluna(nome);
  return CATALOGO_HABITOS_DIARIOS.reduce(function(soma, h){ return soma + (habitosHoje[h.id] ? h.pontos : 0); }, 0);
}

function renderCheckInEHabitos(nome){
  const el = document.getElementById('checkin-habitos-area');
  if(!el) return;
  const checkin = getCheckInHoje(nome);
  const habitosHoje = getHabitosHojeAluna(nome);
  const pontosHoje = calcularPontosHabitosHoje(nome);
  const totalConcluidos = CATALOGO_HABITOS_DIARIOS.filter(function(h){ return habitosHoje[h.id]; }).length;

  function botaoEscala(campo, valorAtual){
    return [1,2,3,4,5].map(function(n){
      return '<span class="chip" style="' + (valorAtual === n ? 'background:var(--gold-soft);color:#1A1409;border-color:var(--gold-soft);' : '') + '" onclick="salvarCampoCheckInEmocional(\'' + nome.replace(/'/g,"\\'") + '\',\'' + campo + '\',' + n + ')">' + n + '</span>';
    }).join('');
  }

  let html = '<p class="section-label" style="margin-top:0;">Como você está hoje?</p>' +
    '<div class="info-box" style="margin-bottom:14px;">' +
      '<p class="txt" style="font-size:12px;margin-bottom:4px;">Humor</p><div class="row" style="gap:6px;margin-bottom:10px;">' + botaoEscala('humor', checkin.humor) + '</div>' +
      '<p class="txt" style="font-size:12px;margin-bottom:4px;">Ansiedade</p><div class="row" style="gap:6px;margin-bottom:10px;">' + botaoEscala('ansiedade', checkin.ansiedade) + '</div>' +
      '<p class="txt" style="font-size:12px;margin-bottom:4px;">Disposição</p><div class="row" style="gap:6px;">' + botaoEscala('disposicao', checkin.disposicao) + '</div>' +
    '</div>' +
    '<div class="info-box" style="margin-bottom:14px;">' +
      '<p class="txt" style="font-size:12px;margin-bottom:4px;">Quantas horas você dormiu?</p>' +
      '<input class="form-input" type="number" step="0.5" min="0" max="14" placeholder="Ex: 7.5" value="' + (checkin.sonoHoras != null ? checkin.sonoHoras : '') + '" onchange="salvarCampoCheckInEmocional(\'' + nome.replace(/'/g,"\\'") + '\',\'sonoHoras\',this.value === \'\' ? null : parseFloat(this.value))" style="margin-bottom:10px;">' +
      '<p class="txt" style="font-size:12px;margin-bottom:4px;">Qualidade do sono</p><div class="row" style="gap:6px;">' + botaoEscala('sonoQualidade', checkin.sonoQualidade) + '</div>' +
    '</div>' +
    '<p class="section-label">Hábitos do dia <span class="tag">' + totalConcluidos + ' de ' + CATALOGO_HABITOS_DIARIOS.length + ' · ' + pontosHoje + ' pts</span></p>';

  CATALOGO_HABITOS_DIARIOS.forEach(function(h){
    const marcado = !!habitosHoje[h.id];
    html += '<div class="list-item" style="margin-bottom:6px;cursor:pointer;' + (marcado ? 'background:var(--success-soft);border-color:var(--success);' : '') + '" onclick="alternarHabitoDoDia(\'' + nome.replace(/'/g,"\\'") + '\',\'' + h.id + '\')">' +
      '<span><i class="ti ' + h.icone + '" style="margin-right:8px;color:' + (marcado ? 'var(--success)' : 'var(--gold-soft)') + ';"></i>' + h.label + '</span>' +
      '<span class="tag" style="' + (marcado ? 'background:var(--success-soft);color:var(--success);' : '') + '">' + (marcado ? '✓ ' : '+') + h.pontos + '</span>' +
    '</div>';
  });

  el.innerHTML = html;
}

// ===== CENTRAL DE AVISOS IN-APP (substitui o WhatsApp por enquanto, sem depender de nenhum serviço externo) =====
function getAvisosInApp(nome){
  const prog = getProgressoAluna(nome);
  if(!prog.avisosInApp) prog.avisosInApp = [];
  return prog.avisosInApp;
}

function enviarAvisoInApp(nomeAluna, texto, tipo){
  const a = alunasPersonal.find(function(x){ return x.nome === nomeAluna; });
  if(!a) return false;
  if(a.aceitaAvisos === false) return false; // respeita quem recusou receber
  const avisos = getAvisosInApp(nomeAluna);
  avisos.unshift({ id: Date.now() + '-' + Math.random().toString(36).slice(2,7), texto: texto, data: new Date().toISOString(), lida: false, tipo: tipo || 'individual' });
  if(avisos.length > 100) avisos.length = 100; // não deixa crescer pra sempre
  salvarProgressoNoSupabase(nomeAluna);
  return true;
}

function contarAvisosNaoLidos(nome){
  return getAvisosInApp(nome).filter(function(av){ return !av.lida; }).length;
}

function renderBadgeAvisos(){
  const badge = document.getElementById('badge-avisos');
  if(!badge) return;
  const naoLidos = contarAvisosNaoLidos(NOME_ALUNA_LOGADA);
  if(naoLidos > 0){ badge.style.display = 'flex'; badge.textContent = naoLidos > 9 ? '9+' : String(naoLidos); }
  else { badge.style.display = 'none'; }
}

function togglePainelAvisos(){
  const painel = document.getElementById('painel-avisos-aluna');
  if(!painel) return;
  const abrindo = painel.style.display === 'none' || !painel.style.display;
  painel.style.display = abrindo ? 'block' : 'none';
  if(abrindo) renderCentralAvisosAluna();
}

function renderCentralAvisosAluna(){
  const painel = document.getElementById('painel-avisos-aluna');
  if(!painel) return;
  const avisos = getAvisosInApp(NOME_ALUNA_LOGADA);
  if(avisos.length === 0){
    painel.innerHTML = '<p class="txt" style="color:var(--text-faint);padding:10px;text-align:center;">Nenhum aviso por aqui ainda.</p>';
    return;
  }
  let html = avisos.length > 1 ? '<div style="text-align:right;margin-bottom:6px;"><span class="chip" style="cursor:pointer;font-size:10px;" onclick="marcarTodosAvisosComoLidos()">Marcar tudo como lido</span></div>' : '';
  html += avisos.map(function(av){
    const dataFormatada = new Date(av.data).toLocaleDateString('pt-BR', { day:'2-digit', month:'2-digit', hour:'2-digit', minute:'2-digit' });
    return '<div class="list-item" style="margin-bottom:6px;flex-direction:column;align-items:flex-start;cursor:pointer;' + (!av.lida ? 'border-color:var(--gold-soft);' : '') + '" onclick="marcarAvisoComoLido(\'' + av.id + '\')">' +
      '<p class="txt" style="font-size:12px;margin:0 0 4px;">' + (!av.lida ? '● ' : '') + av.texto + '</p>' +
      '<p class="txt" style="font-size:10px;color:var(--text-faint);margin:0;">' + dataFormatada + '</p>' +
    '</div>';
  }).join('');
  painel.innerHTML = html;
}

function marcarAvisoComoLido(id){
  const avisos = getAvisosInApp(NOME_ALUNA_LOGADA);
  const aviso = avisos.find(function(av){ return av.id === id; });
  if(aviso) aviso.lida = true;
  salvarProgressoNoSupabase(NOME_ALUNA_LOGADA);
  renderCentralAvisosAluna();
  renderBadgeAvisos();
}

function marcarTodosAvisosComoLidos(){
  getAvisosInApp(NOME_ALUNA_LOGADA).forEach(function(av){ av.lida = true; });
  salvarProgressoNoSupabase(NOME_ALUNA_LOGADA);
  renderCentralAvisosAluna();
  renderBadgeAvisos();
}

// ===== CONSENTIMENTO DE AVISOS (perguntado uma vez, no primeiro acesso) =====
function verificarPerguntarSobreAvisos(){
  const a = alunasPersonal.find(function(x){ return x.nome === NOME_ALUNA_LOGADA; });
  if(!a) return;
  if(a.aceitaAvisos === undefined || a.aceitaAvisos === null){
    const modal = document.getElementById('modal-aceitar-avisos');
    if(modal) modal.style.display = 'flex';
  }
}

function responderAceiteAvisos(aceita){
  const a = alunasPersonal.find(function(x){ return x.nome === NOME_ALUNA_LOGADA; });
  if(a){ a.aceitaAvisos = aceita; salvarPerfilAlunaNoSupabase(a.nome); }
  const modal = document.getElementById('modal-aceitar-avisos');
  if(modal) modal.style.display = 'none';
  renderBadgeAvisos();
}

// ===== ENVIO EM MASSA PELO PERSONAL (in-app, sem depender de WhatsApp/Z-API) =====
function mostrarFormularioAvisoEmMassa(){
  const area = document.getElementById('aviso-massa-inapp-area');
  if(!area) return;
  area.innerHTML =
    '<div class="form-group"><textarea class="form-input" id="texto-aviso-massa-inapp" rows="3" placeholder="Escreva o aviso..."></textarea></div>' +
    '<div class="form-group"><label class="form-label">Pra quem?</label>' +
      '<select class="form-select" id="publico-aviso-massa-inapp">' +
        '<option value="todas">Todas as alunas</option>' +
        '<option value="ativas">Só ativas</option>' +
        '<option value="porvencer">Plano vencendo</option>' +
        '<option value="vencidas">Plano vencido</option>' +
      '</select></div>' +
    '<button class="btn-gold" style="background:var(--card-2);color:var(--gold-soft);border:1px solid var(--border);" onclick="confirmarAvisoEmMassaInApp()">Enviar aviso</button>' +
    '<div id="resultado-aviso-massa-inapp" style="margin-top:10px;"></div>';
}

function confirmarAvisoEmMassaInApp(){
  const texto = document.getElementById('texto-aviso-massa-inapp').value.trim();
  const publico = document.getElementById('publico-aviso-massa-inapp').value;
  if(!texto){ alert('Escreve o aviso antes de enviar.'); return; }

  let elegiveis = alunasPersonal.filter(function(a){ return a.status !== 'lead'; });
  if(publico === 'ativas') elegiveis = elegiveis.filter(function(a){ return statusDoPlano(a) === 'ativas'; });
  if(publico === 'porvencer') elegiveis = elegiveis.filter(function(a){ return statusDoPlano(a) === 'porvencer'; });
  if(publico === 'vencidas') elegiveis = elegiveis.filter(function(a){ return statusDoPlano(a) === 'vencidas'; });

  if(!confirm('Vai mandar esse aviso pra ' + elegiveis.length + ' aluna(s). Continuar?')) return;

  let enviados = 0, recusaram = 0;
  elegiveis.forEach(function(a){
    if(enviarAvisoInApp(a.nome, texto, 'massa')) enviados++; else recusaram++;
  });

  const area = document.getElementById('resultado-aviso-massa-inapp');
  area.innerHTML = '<div class="info-box" style="border-color:var(--success);"><p class="txt">✓ ' + enviados + ' recebida(s)' + (recusaram > 0 ? ' · ' + recusaram + ' não recebem avisos (recusaram no primeiro acesso)' : '') + '</p></div>';
  document.getElementById('texto-aviso-massa-inapp').value = '';
}

// ===== AVISAR TREINO AJUSTADO — mensagem pronta, escolhendo alunas específicas, manda por WhatsApp + in-app juntos =====
const MENSAGEM_PADRAO_TREINO_AJUSTADO = 'Oi! Passando pra avisar que seu treino passou por ajustes e progressões, já está tudo atualizado no app. Dá uma conferida quando puder! 💪';

function mostrarFormularioAvisarTreinoAjustado(){
  const area = document.getElementById('avisar-treino-ajustado-area');
  if(!area) return;
  const elegiveis = alunasPersonal.filter(function(a){ return a.status !== 'lead'; });

  let html = '<div class="form-group"><textarea class="form-input" id="texto-treino-ajustado" rows="3">' + MENSAGEM_PADRAO_TREINO_AJUSTADO + '</textarea></div>';
  html += '<div class="form-group"><input class="form-input" id="busca-treino-ajustado" placeholder="Buscar aluna por nome..." oninput="filtrarListaTreinoAjustado()"></div>';
  html += '<div class="row" style="gap:8px;margin-bottom:8px;"><span class="chip" style="cursor:pointer;" onclick="marcarTodasTreinoAjustado(true)">Selecionar todas</span><span class="chip" style="cursor:pointer;" onclick="marcarTodasTreinoAjustado(false)">Limpar seleção</span></div>';
  html += '<div style="max-height:220px;overflow-y:auto;border:1px solid var(--border);border-radius:10px;padding:8px;margin-bottom:10px;">';
  html += elegiveis.map(function(a){
    return '<label class="linha-treino-ajustado" data-nome-busca="' + a.nome.toUpperCase().replace(/"/g,'&quot;') + '" style="display:flex;align-items:center;gap:8px;padding:6px 4px;font-size:13px;cursor:pointer;">' +
      '<input type="checkbox" class="checkbox-treino-ajustado" value="' + a.nome.replace(/"/g,'&quot;') + '"> ' + a.nome +
    '</label>';
  }).join('');
  html += '<p id="sem-resultado-treino-ajustado" class="txt" style="display:none;color:var(--text-faint);text-align:center;padding:8px;">Nenhuma aluna encontrada.</p>';
  html += '</div>';
  html += '<button class="btn-gold" style="background:var(--card-2);color:var(--gold-soft);border:1px solid var(--border);" onclick="confirmarAvisarTreinoAjustado()">Enviar (WhatsApp + app)</button>';
  html += '<div id="resultado-treino-ajustado" style="margin-top:10px;"></div>';
  area.innerHTML = html;
}

function filtrarListaTreinoAjustado(){
  const termo = (document.getElementById('busca-treino-ajustado').value || '').toUpperCase().trim();
  let visiveis = 0;
  document.querySelectorAll('.linha-treino-ajustado').forEach(function(linha){
    const bate = !termo || linha.getAttribute('data-nome-busca').indexOf(termo) !== -1;
    linha.style.display = bate ? 'flex' : 'none';
    if(bate) visiveis++;
  });
  const semResultado = document.getElementById('sem-resultado-treino-ajustado');
  if(semResultado) semResultado.style.display = visiveis === 0 ? 'block' : 'none';
}

function marcarTodasTreinoAjustado(marcar){
  // Só marca/desmarca quem está visível na busca atual, pra não mexer sem querer em quem está escondido pelo filtro
  document.querySelectorAll('.linha-treino-ajustado').forEach(function(linha){
    if(linha.style.display === 'none') return;
    const chk = linha.querySelector('.checkbox-treino-ajustado');
    if(chk) chk.checked = marcar;
  });
}

async function confirmarAvisarTreinoAjustado(){
  const texto = document.getElementById('texto-treino-ajustado').value.trim();
  if(!texto){ alert('Escreve a mensagem antes de enviar.'); return; }
  const nomesMarcados = Array.from(document.querySelectorAll('.checkbox-treino-ajustado:checked')).map(function(el){ return el.value; });
  if(nomesMarcados.length === 0){ alert('Marca pelo menos uma aluna.'); return; }
  if(!confirm('Vai mandar essa mensagem por WhatsApp e pelo app pra ' + nomesMarcados.length + ' aluna(s). Continuar?')) return;

  const area = document.getElementById('resultado-treino-ajustado');
  let enviadosWhats = 0, errosWhats = [], enviadosApp = 0;

  for(let i = 0; i < nomesMarcados.length; i++){
    const nome = nomesMarcados[i];
    const a = alunasPersonal.find(function(x){ return x.nome === nome; });
    if(!a) continue;
    area.innerHTML = '<p class="txt" style="color:var(--text-faint);">Enviando ' + (i+1) + ' de ' + nomesMarcados.length + '...</p>';
    if(enviarAvisoInApp(a.nome, texto, 'ajuste_treino')) enviadosApp++;
    if(a.telefone){
      const saida = await enviarWhatsApp(a.telefone, texto);
      if(saida.sucesso) enviadosWhats++; else errosWhats.push({ nome: a.nome, motivo: saida.motivo });
    } else {
      errosWhats.push({ nome: a.nome, motivo: 'sem telefone cadastrado' });
    }
    await new Promise(function(r){ setTimeout(r, 150); });
  }

  const listaErros = errosWhats.length ? '<ul style="margin:8px 0 0;padding-left:18px;font-size:11px;color:var(--text-faint);">' + errosWhats.map(function(e){ return '<li>' + e.nome + ': ' + (e.motivo || 'motivo não informado') + '</li>'; }).join('') + '</ul>' : '';
  area.innerHTML = '<div class="info-box" style="border-color:' + (errosWhats.length ? '#C9784A' : 'var(--success)') + ';">' +
    '<p class="txt">✓ ' + enviadosApp + ' recebida(s) no app · ' + enviadosWhats + ' recebida(s) no WhatsApp' + (errosWhats.length ? ' · ' + errosWhats.length + ' com erro no WhatsApp' : '') + '</p>' +
    listaErros +
  '</div>';
}

function escolherSemRepetirUltima(lista, prog, chave){
  if(!prog.ultimaFraseUsada) prog.ultimaFraseUsada = {};
  const ultimaIdx = prog.ultimaFraseUsada[chave];
  let opcoes = lista.map(function(item, i){ return i; });
  if(ultimaIdx !== undefined && opcoes.length > 1) opcoes = opcoes.filter(function(i){ return i !== ultimaIdx; });
  const escolhidaIdx = opcoes[Math.floor(Math.random() * opcoes.length)];
  prog.ultimaFraseUsada[chave] = escolhidaIdx;
  return lista[escolhidaIdx];
}

function gerarRespostaCheckIn(nome, vaiTreinar){
  const primeiroNome = nome.split(' ')[0];
  const prog = getProgressoAluna(nome);
  const respostas = registrarRespostaCheckIn(nome, vaiTreinar);
  const sequencia = calcularSequenciaCheckIn(respostas);

  if(vaiTreinar){
    if(sequencia >= 3){
      const variasBoas = [
        { t: 'Isso aí, ' + primeiroNome + '! Já são ' + sequencia + ' seguidas sem deixar passar. É exatamente essa constância que traz resultado.', p: false },
        { t: primeiroNome + ', olha essa sequência! ' + sequencia + ' treinos seguidos confirmados. Continua nesse ritmo que a evolução vem.', p: false },
        { t: 'Você tá numa fase muito boa, ' + primeiroNome + '. ' + sequencia + ' seguidos! Isso é o que separa quem fala de quem faz.', p: false },
        { t: primeiroNome + ', ' + sequencia + ' treinos direto, sem falhar. Continua exatamente assim.', p: false },
        { t: 'Gostei de ver, ' + primeiroNome + '. Constância desse jeito é o que vira resultado de verdade lá na frente.', p: false }
      ];
      return escolherSemRepetirUltima(variasBoas, prog, 'sim_sequencia');
    }
    const variasSimples = [
      { t: 'Boa! Treino já está pronto pra você, bora.', p: false },
      { t: 'Isso, ' + primeiroNome + '! Te espero lá.', p: false },
      { t: 'Show, ' + primeiroNome + '. Vamos que vamos.', p: false },
      { t: 'Perfeito, treino liberado. Bom treino!', p: false },
      { t: primeiroNome + ', gostei. Bora fazer valer.', p: false }
    ];
    return escolherSemRepetirUltima(variasSimples, prog, 'sim_simples');
  } else {
    if(sequencia >= 3){
      const variasPadraoFalta = [
        { t: primeiroNome + ', já são ' + sequencia + ' vezes seguidas. Sei que a rotina aperta de verdade, mas pra chegar onde você quer, precisa ter compromisso com isso. Bora retomar hoje, nem que seja curto?', p: true },
        { t: 'Percebi que faz um tempo que você não confirma, ' + primeiroNome + '. Continuo aqui do seu lado, mas resultado exige constância, não só querer. Que tal começar de novo hoje?', p: true },
        { t: primeiroNome + ', entendo que a semana pode ter apertado, mas ' + sequencia + ' vezes seguidas já é um padrão. O que pode te ajudar a voltar hoje?', p: true },
        { t: 'Sem cobrança vazia, ' + primeiroNome + ', mas preciso ser direto: sem retomar agora, fica difícil chegar no resultado que você quer. Vamos hoje?', p: true },
        { t: primeiroNome + ', ' + sequencia + ' faltas seguidas mudam o resultado final, não só o treino de hoje. Bora reverter isso agora?', p: true }
      ];
      return escolherSemRepetirUltima(variasPadraoFalta, prog, 'nao_padrao');
    }
    const variasPrimeiraFalta = [
      { t: 'Tudo bem, descansar às vezes também importa. Só não esquece: resultado vem de constância, não de treino perfeito. Te vejo amanhã?', p: true },
      { t: 'Sem problema, ' + primeiroNome + '. Só lembrando que é a repetição que constrói o resultado que você quer, não um dia isolado. Amanhã a gente retoma?', p: true },
      { t: 'Tranquilo, ' + primeiroNome + '. Um dia não define nada, mas não deixa virar dois. Amanhã tem treino?', p: true },
      { t: 'Beleza, descanso de vez em quando faz parte. Só não deixa esfriar, viu? Amanhã a gente encara?', p: true },
      { t: primeiroNome + ', tudo bem por hoje. Só lembra que resultado é feito de dias como amanhã. Confirma pra mim?', p: true }
    ];
    return escolherSemRepetirUltima(variasPrimeiraFalta, prog, 'nao_primeira');
  }
}

function gerarRespostaFollowUp(nome, vaiTreinar){
  const primeiroNome = nome.split(' ')[0];
  const prog = getProgressoAluna(nome);
  registrarRespostaCheckIn(nome, vaiTreinar);
  if(vaiTreinar){
    const opcoes = [
      { t: 'Isso! Contando com você.', p: false },
      { t: 'Show, é isso aí.', p: false },
      { t: 'Combinado, ' + primeiroNome + '.', p: false }
    ];
    return escolherSemRepetirUltima(opcoes, prog, 'followup_sim').t;
  } else {
    const opcoes = [
      { t: 'Tudo bem. Qualquer coisa, tô por aqui.', p: false },
      { t: 'Ok, ' + primeiroNome + '. Sem drama, seguimos.', p: false },
      { t: 'Entendido. Te vejo em breve.', p: false }
    ];
    return escolherSemRepetirUltima(opcoes, prog, 'followup_nao').t;
  }
}


function simularCenarioCheckIn(cenario){
  const nome = 'Andriele Caroline Rubert';
  const prog = getProgressoAluna(nome);
  prog.respostasCheckIn = [];

  if(cenario === 'sequenciaBoa'){
    prog.respostasCheckIn = [{vaiTreinar:true},{vaiTreinar:true},{vaiTreinar:true}];
    document.getElementById('banner-notif-titulo').textContent = 'Oi, Andriele! 💛';
    document.getElementById('banner-notif-corpo').textContent = 'Você respondeu "sim" 3 vezes seguidas até agora. Vai treinar de novo hoje?';
  } else if(cenario === 'primeiraFalta'){
    prog.respostasCheckIn = [{vaiTreinar:true}];
    document.getElementById('banner-notif-titulo').textContent = 'Oi, Andriele! 💛';
    document.getElementById('banner-notif-corpo').textContent = 'Hoje costuma ser dia de treino seu. Vai treinar?';
  } else if(cenario === 'padraoSumiço'){
    prog.respostasCheckIn = [{vaiTreinar:false},{vaiTreinar:false},{vaiTreinar:false}];
    document.getElementById('banner-notif-titulo').textContent = 'Oi, Andriele! 💛';
    document.getElementById('banner-notif-corpo').textContent = 'Faz uns dias que não te vejo por aqui. Vai treinar hoje?';
  }
  resetarBotoesCheckIn();
  document.getElementById('banner-notificacao-simulada').style.display = 'block';
}

function simularNotificacaoCheckIn(){
  const nome = NOME_ALUNA_LOGADA; // funciona pra qualquer aluna logada, não só a de teste
  const padrao = detectarPadraoTreino(nome);
  const texto = gerarTextoNotificacaoCheckIn(nome);
  document.getElementById('banner-notif-titulo').textContent = texto.titulo;
  document.getElementById('banner-notif-corpo').textContent = texto.corpo;
  resetarBotoesCheckIn();
  document.getElementById('banner-notificacao-simulada').style.display = 'block';
  console.log('[Simulação] Padrão detectado:', padrao);
}

function responderCheckIn(vaiTreinar){
  const banner = document.getElementById('banner-notificacao-simulada');
  const corpo = document.getElementById('banner-notif-corpo');
  const botoes = document.getElementById('banner-notif-botoes');
  const resposta = gerarRespostaCheckIn(NOME_ALUNA_LOGADA, vaiTreinar);
  corpo.textContent = resposta.t;

  if(resposta.p){
    // Termina em pergunta, mantém os botões pra ela responder de novo
    botoes.innerHTML =
      '<span class="chip" style="cursor:pointer;flex:1;text-align:center;" onclick="responderFollowUpCheckIn(true)">Sim</span>' +
      '<span class="chip" style="cursor:pointer;flex:1;text-align:center;" onclick="responderFollowUpCheckIn(false)">Não</span>';
    botoes.style.display = 'flex';
  } else {
    botoes.style.display = 'none';
    setTimeout(function(){ banner.style.display = 'none'; resetarBotoesCheckIn(); }, 4000);
  }
}

function responderFollowUpCheckIn(vaiTreinar){
  const banner = document.getElementById('banner-notificacao-simulada');
  const corpo = document.getElementById('banner-notif-corpo');
  const botoes = document.getElementById('banner-notif-botoes');
  corpo.textContent = gerarRespostaFollowUp(NOME_ALUNA_LOGADA, vaiTreinar);
  botoes.style.display = 'none';
  setTimeout(function(){ banner.style.display = 'none'; resetarBotoesCheckIn(); }, 3500);
}

function resetarBotoesCheckIn(){
  const botoes = document.getElementById('banner-notif-botoes');
  botoes.innerHTML =
    '<span class="chip" style="cursor:pointer;flex:1;text-align:center;" onclick="responderCheckIn(true)">Sim, vou treinar</span>' +
    '<span class="chip" style="cursor:pointer;flex:1;text-align:center;" onclick="responderCheckIn(false)">Hoje não</span>';
  botoes.style.display = 'flex';
}

function gerarTextoNotificacaoCheckIn(nome){
  const padrao = detectarPadraoTreino(nome);
  const primeiroNome = nome.split(' ')[0];
  if(!padrao.temPadrao){
    return { titulo: 'Oi, ' + primeiroNome + '! 💛', corpo: 'Vai treinar hoje? Toque pra abrir seu treino do dia.' };
  }
  const diaHoje = ['Domingo','Segunda','Terça','Quarta','Quinta','Sexta','Sábado'][new Date().getDay()];
  const ehDiaTypico = padrao.diasTipicos.indexOf(diaHoje) !== -1;
  const corpo = ehDiaTypico
    ? 'Hoje costuma ser dia de treino seu, por volta da ' + padrao.periodoDia + '. Vai treinar?'
    : 'Notei que você costuma treinar mais em ' + padrao.diasTipicos.join('/') + ', mas hoje também vale! Vai treinar?';
  return { titulo: 'Oi, ' + primeiroNome + '! 💛', corpo: corpo };
}

function totalDiasDeTreino(){
  return dias.filter(function(d){ return !d.descanso; }).length;
}

function checarConclusaoSemana(nomeAluna){
  const prog = getProgressoAluna(nomeAluna);
  const total = totalDiasDeTreino();
  const concluidos = (prog.diasConcluidos[prog.semana] || []).length;
  if(concluidos >= total){
    prog.semana += 1;
    return { avancou: true, concluidos: concluidos, total: total };
  }
  return { avancou: false, concluidos: concluidos, total: total };
}

const JANELA_MINIMA_FASE_SEMANAS = 10; // 2-3 meses, com base na literatura de correção postural (efeito mensurável em 4-6 semanas)

function calcularElegibilidadeFase(nome){
  const a = alunasPersonal.find(function(x){ return x.nome === nome; });
  if(!a || !a.treinoAtual) return null;
  const prog = getProgressoAluna(nome);
  const stats = calcularEstatisticasAluna(nome);
  const nutriStats = calcularNutricaoStats(nome);
  const semanasNaFase = prog.semana; // fase começou na semana 1 pra Andriele

  const criterios = [];
  criterios.push({
    nome: 'Tempo mínimo na fase (2-3 meses)',
    atingido: semanasNaFase >= JANELA_MINIMA_FASE_SEMANAS,
    detalhe: semanasNaFase + ' de ' + JANELA_MINIMA_FASE_SEMANAS + ' semanas'
  });
  const pctConstancia = stats.temDados ? Math.round((stats.totalConcluidos / stats.totalPlanejado) * 100) : 0;
  criterios.push({
    nome: 'Constância ≥ 70%',
    atingido: pctConstancia >= 70,
    detalhe: pctConstancia + '%'
  });
  if(a.treinoAtual.fase.indexOf('Emagrecimento') !== -1){
    criterios.push({
      nome: 'Resposta Nutricional ≥ 60',
      atingido: nutriStats.temDados && nutriStats.respostaNutricional >= 60,
      detalhe: nutriStats.temDados ? nutriStats.respostaNutricional : 'sem dados'
    });
    const pesoInicial = extrairPesoKg(a.peso);
    const historico = a.pesoHistorico || [];
    const pesoAtual = historico.length ? historico[historico.length - 1].peso : pesoInicial;
    const percPerdido = pesoInicial ? Math.round(((pesoInicial - pesoAtual) / pesoInicial) * 1000) / 10 : 0;
    criterios.push({
      nome: 'Perda de peso registrada (≥ 3%)',
      atingido: percPerdido >= 3,
      detalhe: percPerdido + '% (' + pesoInicial + 'kg → ' + pesoAtual + 'kg)'
    });
  } else if(a.treinoAtual.fase.indexOf('postural') !== -1){
    criterios.push({
      nome: 'Confirmação visual do personal (nova foto)',
      atingido: !!a.fotoReavaliada,
      detalhe: a.fotoReavaliada ? 'Confirmada' : 'Pendente'
    });
  }

  const elegivel = criterios.every(function(c){ return c.atingido; });
  return { elegivel: elegivel, criterios: criterios };
}

function verificarCheckinPeso(nomeAluna){
  const prog = getProgressoAluna(nomeAluna);
  const mesAtual = Math.floor((prog.semana - 1) / 4) + 1;
  if(!prog.pesoChecks) prog.pesoChecks = {};
  if(prog.semana > 1 && (prog.semana - 1) % 4 === 0 && !prog.pesoChecks[mesAtual - 1]){
    return mesAtual - 1;
  }
  return null;
}

function renderPerguntaPeso(mes){
  return '<div class="info-box" id="area-peso" style="margin-top:10px;">' +
    '<p class="lbl">Check-in mensal de peso</p>' +
    '<p class="txt">Já faz um mês! Pode me passar seu peso atual? Isso ajuda a calibrar quando é hora de avançar de fase.</p>' +
    '<div class="form-group"><input class="form-input" id="peso-atual-input" type="number" step="0.1" placeholder="Ex: 98.5"></div>' +
    '<button class="btn-gold" style="background:var(--card-2);color:var(--gold-soft);border:1px solid var(--border);" onclick="registrarPesoMensal(' + mes + ')">Enviar</button>' +
    '</div>';
}

function registrarPesoMensal(mes){
  const alunaAtual = alunasPersonal.find(function(a){ return a.nome === NOME_ALUNA_LOGADA; });
  if(!alunaAtual) return;
  const prog = getProgressoAluna(alunaAtual.nome);
  const peso = parseFloat(document.getElementById('peso-atual-input').value);
  if(isNaN(peso)) return;
  if(!prog.pesoChecks) prog.pesoChecks = {};
  prog.pesoChecks[mes] = true;
  if(!alunaAtual.pesoHistorico) alunaAtual.pesoHistorico = [];
  alunaAtual.pesoHistorico.push({ semana: prog.semana, peso: peso });
  const el = document.getElementById('area-peso');
  if(el) el.innerHTML = '<p class="txt">Peso registrado! Isso já entra no cálculo de elegibilidade de mudança de fase.</p>';
}

function extrairExercicios(a){
  const nomes = [];
  if(a.treinoAtual){
    a.treinoAtual.dias.forEach(function(d){
      d.ex.forEach(function(linha){
        const nome = linha.split(' · ')[0];
        if(nomes.indexOf(nome) === -1) nomes.push(nome);
      });
    });
  }
  return nomes;
}

function sugerirAjusteCarga(carga, reps){
  if(reps >= 12) return { texto: 'Aumentar carga', valor: Math.round((carga * 1.08) * 2) / 2 };
  if(reps >= 10) return { texto: 'Manter, pequeno aumento no próximo ciclo', valor: carga };
  if(reps >= 8) return { texto: 'Manter, repetir', valor: carga };
  if(reps >= 6) return { texto: 'Pequena redução', valor: Math.round((carga * 0.95) * 2) / 2 };
  return { texto: 'Redução mais significativa', valor: Math.round((carga * 0.90) * 2) / 2 };
}

function registrarSessao(){
  const nome = document.getElementById('prog-exercicio').value;
  const carga = parseFloat(document.getElementById('prog-carga').value);
  const reps = parseInt(document.getElementById('prog-reps').value, 10);
  if(!nome || isNaN(carga) || isNaN(reps)) return;
  const prog = getProgressoAluna(alunaAberta.nome);
  if(!prog.historico[nome]) prog.historico[nome] = [];
  const sugestao = sugerirAjusteCarga(carga, reps);
  prog.historico[nome].push({ semana: prog.semana, carga: carga, reps: reps, sugestao: sugestao });
  const i = alunasPersonal.indexOf(alunaAberta);
  openAlunaDetail(i);
}

function avancarSemana(){
  const resultado = checarConclusaoSemana(alunaAberta.nome);
  const i = alunasPersonal.indexOf(alunaAberta);
  openAlunaDetail(i);
  const conf = document.getElementById('prog-reengajamento');
  if(!resultado.avancou && conf){
    conf.innerHTML = '<div class="insight"><p><b>Semana não concluída</b> (' + resultado.concluidos + '/' + resultado.total + ' treinos), a semana não avança, e o sistema já disparou uma mensagem de check-in pra aluna, avisando você em paralelo.</p></div>' +
      '<div class="form-group"><label class="form-label">Motivo relatado (opcional, pra seu controle)</label><input class="form-input" id="motivo-falta" placeholder="Ex: imprevisto de trabalho"></div>';
  } else if(resultado.avancou && conf){
    const prog = getProgressoAluna(alunaAberta.nome);
    const semanaFechada = prog.semana - 1;
    let extra = '';
    if(!prog.nutricao || !prog.nutricao[semanaFechada]){
      extra += renderPerguntaNutricao(semanaFechada);
    }
    const mesPendente = verificarCheckinPeso(alunaAberta.nome);
    if(mesPendente !== null){
      extra += renderPerguntaPeso(mesPendente);
    }
    conf.innerHTML = '<div class="insight" id="nutri-confirmacao"><p>Semana concluída!' + (extra ? ' Ainda faltam alguns check-ins dessa semana.' : '') + '</p></div>' + extra;
  }
}

function avancarSemanaForcado(){
  const prog = getProgressoAluna(alunaAberta.nome);
  prog.semana += 1;
  const i = alunasPersonal.indexOf(alunaAberta);
  openAlunaDetail(i);
}

/* ===== ITEM 1: Volume de transição (aluna que pausou e retomou) ===== */
const VOLUME_TRANSICAO = 22; // entre o teto de iniciante (20) e o início de intermediário (25-30)
const SEMANAS_TRANSICAO = 4;

function calcularTetoVolume(nivel, semanaAtual, pausouRetomou){
  if(pausouRetomou && semanaAtual <= SEMANAS_TRANSICAO){
    return { teto: VOLUME_TRANSICAO, emTransicao: true };
  }
  const tetos = { 'Iniciante': 20, 'Intermediário': 30, 'Avançado': 50 };
  return { teto: tetos[nivel] || 20, emTransicao: false };
}

/* ===== ITEM 2 e 3: Estimativa de duração de treino (com regra de unilateral) ===== */
function estimarDuracaoExercicio(series, descansoTexto, unilateral){
  const descansoSegundos = descansoParaSegundos(descansoTexto);
  const execucaoPorSerie = 40; // segundos, média
  let segundosPorSerie;
  if(unilateral){
    // perna 1 (~40s) + descanso curto entre pernas (25s) + perna 2 (~40s) + descanso completo
    segundosPorSerie = 40 + 25 + 40 + descansoSegundos;
  } else {
    segundosPorSerie = execucaoPorSerie + descansoSegundos;
  }
  return Math.round((series * segundosPorSerie) / 60 * 10) / 10; // minutos
}

const PALAVRAS_UNILATERAL = ['UNILATERAL', 'JOELHOS EM PÉ', 'NA POLIA', 'AFUNDO', 'BÚLGARO', 'BULGARO', 'STEP UP'];
function ehExercicioUnilateral(nomeExercicio){
  const upper = nomeExercicio.toUpperCase();
  return PALAVRAS_UNILATERAL.some(function(p){ return upper.indexOf(p) !== -1; });
}

/* ===== ITEM 4: Volume do Bloco de Choque (corte de 15-20%, nunca 40-50%) ===== */
function calcularVolumeChoque(volumeBase){
  return Math.round(volumeBase * 0.82); // ~18% de corte, dentro da faixa 15-20%
}

/* ===== ITEM 5: Blocos de cluster set variáveis ===== */
const BLOCOS_CLUSTER_SET = [3, 4, 6, 8, 10];
function sugerirBlocoCluster(indiceCiclo){
  return BLOCOS_CLUSTER_SET[indiceCiclo % BLOCOS_CLUSTER_SET.length];
}

/* ===== ITEM 6: Multiarticulares contam volume duplo (Quadríceps + Glúteo) ===== */
const MULTIARTICULARES_INFERIORES = ['AGACHAMENTO', 'LEG PRESS', 'HACK', 'BÚLGARO', 'BULGARO', 'AFUNDO', 'STEP UP'];
function ehMultiarticularInferior(nomeExercicio){
  const upper = nomeExercicio.toUpperCase();
  return MULTIARTICULARES_INFERIORES.some(function(k){ return upper.indexOf(k) !== -1; });
}
function temAmplitudeElevada(nomeExercicio){
  const upper = nomeExercicio.toUpperCase();
  return upper.indexOf('PROFUNDO') !== -1 || upper.indexOf('AMPLITUDE ELEVADA') !== -1 || upper.indexOf('AMPLITUDE COMPLETA') !== -1;
}
function contaVolumeDuplo(nomeExercicio){
  return ehMultiarticularInferior(nomeExercicio) && temAmplitudeElevada(nomeExercicio);
}

/* ===== ITEM 7: Restpause e Cluster set nunca empilhados no mesmo exercício ===== */
function validarMetodoUnico(metodoTexto){
  const upper = (metodoTexto || '').toUpperCase();
  const temRestpause = upper.indexOf('RESTPAUSE') !== -1;
  const temCluster = upper.indexOf('CLUSTER') !== -1;
  if(temRestpause && temCluster){
    return { valido: false, motivo: 'Restpause e Cluster set nunca são usados juntos na mesma série, escolha um método por vez (exceção: avançada, só na última série).' };
  }
  return { valido: true, motivo: '' };
}

/* ===== ITEM 8: Direcionamento de quadríceps (região distal) ===== */
function sugerirExerciciosQuadriceps(direcionamento){
  if(direcionamento === 'distal'){
    return {
      prioridade: ['Cadeira Extensora', 'Flexão Nórdica Reversa'],
      nota: 'Flexão Nórdica Reversa ainda não está cadastrada no banco, pendente de cadastro.',
      estrategia: 'isolados'
    };
  }
  return {
    prioridade: MULTIARTICULARES_INFERIORES,
    nota: 'Sem necessidade de região distal, mantém estratégia de multiarticulares com boa amplitude (contam duplo pra Glúteo também).',
    estrategia: 'multiarticulares'
  };
}

/* ===== ITEM 9: Direcionamento de glúteo (região inferior/superior) ===== */
const EXERCICIOS_GLUTEO_INFERIOR = ['Afundo', 'Agachamento Búlgaro', 'Step Up', 'Extensão de quadril no banco romano', 'Coice (glúteo na polia)'];
const EXERCICIOS_GLUTEO_SUPERIOR = ['Extensão de quadril com perna estendida', 'Rotação de quadril', 'Abdução de quadril na polia', 'Cadeira Abdutora em pé'];

function sugerirExerciciosGluteo(direcionamento){
  if(direcionamento === 'inferior') return { prioridade: EXERCICIOS_GLUTEO_INFERIOR, regiao: 'Região inferior (próxima à prega glútea)' };
  if(direcionamento === 'superior') return { prioridade: EXERCICIOS_GLUTEO_SUPERIOR, regiao: 'Região superior/lateral (glúteo médio)' };
  if(direcionamento === 'ambas') return { prioridade: EXERCICIOS_GLUTEO_INFERIOR.concat(EXERCICIOS_GLUTEO_SUPERIOR), regiao: 'Ambas as regiões' };
  return { prioridade: EXERCICIOS_GLUTEO_INFERIOR.concat(EXERCICIOS_GLUTEO_SUPERIOR).slice(0, 4), regiao: 'Sem prioridade específica, mix padrão' };
}

/* ===== ITEM 10: Reforço cruzado (grupo carente recebe volume leve em dia do "outro grupo") ===== */
function sugerirReforcoCruzado(nomeExercicioLeve){
  return nomeExercicioLeve + ' · 4x15-20 (carga baixa, longe da falha, estímulo, não sobrecarga)';
}

/* ===== ITEM 11: Famílias de movimento, rotação entre ciclos ===== */
const FAMILIAS_MOVIMENTO = {
  'puxada_horizontal': ['Remada Articular Aberta', 'Remada Articular Fechada', 'Remada Curvada com Halteres', 'Crucifixo Inverso', 'Remada Baixa'],
  'agachamento': ['Agachamento Hack', 'Leg Press 45', 'Agachamento Livre'],
  'flexao_cotovelo': ['Rosca Direta', 'Rosca Alternada com Halteres', 'Rosca Martelo'],
  'extensao_cotovelo': ['Tríceps Corda', 'Tríceps Testa', 'Tríceps Francês']
};

function obterFamiliaDoExercicio(nomeExercicio){
  const upper = nomeExercicio.toUpperCase();
  for(const familia in FAMILIAS_MOVIMENTO){
    if(FAMILIAS_MOVIMENTO[familia].some(function(e){ return e.toUpperCase() === upper; })){
      return familia;
    }
  }
  return null;
}

function rotacionarExercicio(nomeExercicioAtual, ehAncora, indiceCiclo){
  if(ehAncora) return nomeExercicioAtual;
  const familia = obterFamiliaDoExercicio(nomeExercicioAtual);
  if(!familia) return nomeExercicioAtual; // sem família mapeada ainda, mantém como está
  const opcoes = FAMILIAS_MOVIMENTO[familia];
  const ciclo = indiceCiclo || 0;
  return opcoes[ciclo % opcoes.length];
}

/* ===== ITEM 12: Vaga com 1 exercício carregando o volume, ou 2 dividindo o mesmo volume ===== */
function dividirVolumeEmDois(nomeExercicio, seriesTotal, repsAlvo){
  const familia = obterFamiliaDoExercicio(nomeExercicio);
  if(!familia || FAMILIAS_MOVIMENTO[familia].length < 2) return null;
  const opcoes = FAMILIAS_MOVIMENTO[familia];
  const ex1 = opcoes[0];
  const ex2 = opcoes[1] !== ex1 ? opcoes[1] : opcoes[2];
  const seriesCada = Math.max(1, Math.round(seriesTotal / 2));
  return [
    ex1 + ' · ' + seriesCada + 'x' + repsAlvo,
    ex2 + ' · ' + (seriesTotal - seriesCada) + 'x' + repsAlvo
  ];
}

/* ===== ITEM 13: O GERADOR ===== */

// Um exercício "serve" pra casa se já for marcado ambiente Casa, OU se usar equipamento portátil
// (barra, halteres, elástico) mesmo estando catalogado como Academia — dá pra levar pra casa.
function exercicioServeParaCasa(e){
  if((e.ambiente || 'Academia') === 'Casa') return true;
  const n = e.nome.toUpperCase();
  // Mesmo citando barra/halteres/elástico, se também citar equipamento fixo de academia, não serve pra casa
  const ehEquipamentoFixo = ['POLIA', 'MÁQUINA', 'MAQUINA', 'FIXA', 'SMITH', 'HACK', 'LEG PRESS'].some(function(p){ return n.indexOf(p) !== -1; });
  if(ehEquipamentoFixo) return false;
  return n.indexOf('BARRA') !== -1 || n.indexOf('HALTERE') !== -1 || n.indexOf('ELÁSTICO') !== -1 || n.indexOf('ELASTICO') !== -1;
}

// Multiarticular = movimento composto, envolve mais de uma articulação principal.
// Regra da metodologia: elástico é acessório/isolamento em academia — multiarticular com elástico não entra
// em treino de academia (lá tem equipamento de verdade pra isso). Em casa continua liberado normalmente.
const padroesMultiarticulares = ['AGACHAMENTO', 'TERRA', 'AFUNDO', 'REMADA', 'PUXADA', 'DESENVOLVIMENTO', 'SUPINO', 'LEVANTAMENTO', 'PASSADA'];
function ehMultiarticularComElastico(e){
  const n = e.nome.toUpperCase();
  const temElastico = n.indexOf('ELÁSTICO') !== -1 || n.indexOf('ELASTICO') !== -1;
  if(!temElastico) return false;
  return padroesMultiarticulares.some(function(p){ return n.indexOf(p) !== -1; });
}

// Elástico nunca entra em treino de Academia (lá tem equipamento de verdade pra isso), com uma única
// exceção: o bloco de mobilidade/correção postural, que é gerado por uma lógica totalmente separada
// (obterBlocoPostural) e não passa por aqui. Em treino de Casa, elástico continua liberado normalmente.
function linhaUsaElastico(linha){
  const upper = linha.toUpperCase();
  return upper.indexOf('ELÁSTICO') !== -1 || upper.indexOf('ELASTICO') !== -1;
}

// Pliométrico = salto/impacto/explosão — carga alta e repentina no joelho. Regra da metodologia:
// quem relata dor no joelho não recebe esse tipo de exercício, seja qual for o grupo muscular.
function ehExercicioPliometrico(e){
  const n = e.nome.toUpperCase();
  return ['SALTO', 'JUMP', 'PULO', 'POLICHINELO', 'SKIPPING', 'VAI E VEM', 'PLIOMÉTRIC', 'PLIOMETRIC'].some(function(p){ return n.indexOf(p) !== -1; });
}

// Elástico é sempre isolamento/acessório, próprio pra treino em Casa (onde não tem equipamento de
// verdade). Em Academia nunca entra, seja isolado ou multiarticular — lá tem equipamento de verdade
// pra cada estímulo. Única exceção real: o bloco de mobilidade/correção postural (obterBlocoPostural),
// que é uma lógica totalmente separada e não passa por aqui.
function ehExercicioElastico(e){
  const n = e.nome.toUpperCase();
  return e.metodo === 'Elástico' || n.indexOf('ELÁSTICO') !== -1 || n.indexOf('ELASTICO') !== -1;
}

function obterPoolRealDoGrupo(grupoInterno, ambiente, evitarPliometrico){
  const mapaGrupo = { 'Glúteo': 'Glúteos', 'Posterior': 'Isquiotibiais', 'Quadríceps': 'Quadríceps', 'Ombro': 'Ombros', 'Peito': 'Peito', 'Bíceps': 'Bíceps', 'Tríceps': 'Tríceps', 'Costas': 'Costas' };
  const grupoReal = mapaGrupo[grupoInterno] || grupoInterno;
  // Regra fixa da metodologia: treino de academia nunca recebe exercício de casa, e vice-versa —
  // mas pra Casa, também libera exercício de barra/halteres/elástico, mesmo catalogado como Academia
  const baseAmbiente = exerciciosBanco.filter(function(e){
    if(e.grupo !== grupoReal) return false;
    if(evitarPliometrico && ehExercicioPliometrico(e)) return false;
    return ambiente === 'Casa' ? exercicioServeParaCasa(e) : ((e.ambiente || 'Academia') === 'Academia' && !ehExercicioElastico(e));
  });
  const comVideo = baseAmbiente.filter(function(e){ return e.video; });
  return comVideo.length >= 3 ? comVideo : baseAmbiente;
}

function hashString(str){
  let hash = 0;
  for(let i = 0; i < str.length; i++){ hash = (hash * 31 + str.charCodeAt(i)) % 1000; }
  return hash;
}

function capitalizarNomeExercicio(nome){
  return nome.split(' ').map(function(palavra){
    if(palavra.length <= 2) return palavra.toLowerCase(); // preposições curtas (de, em, na...)
    return palavra.charAt(0) + palavra.slice(1).toLowerCase();
  }).join(' ');
}

function obterPalavraChaveMovimento(nomeExercicio){
  const upper = nomeExercicio.toUpperCase();
  const padroesConhecidos = ['STIFF', 'CADEIRA', 'ELEVAÇÃO', 'EXTENSÃO', 'AFUNDO', 'AGACHAMENTO', 'FLEXÃO', 'ABDUÇÃO', 'ADUÇÃO', 'LEG PRESS', 'HACK', 'TERRA', 'BOM DIA', 'COICE', 'GLÚTEO', 'PONTE'];
  for(let i = 0; i < padroesConhecidos.length; i++){
    if(upper.indexOf(padroesConhecidos[i]) !== -1) return padroesConhecidos[i];
  }
  return upper.split(' ')[0]; // fallback: primeira palavra do nome
}

// ===== MOTOR 5: FAMÍLIA BIOMECÂNICA =====
// Agrupa exercícios pelo padrão de movimento dominante (não só pela palavra no nome).
// Ex: Hip Thrust, Glúteo 4 Apoios, Ponte de Glúteo e Coice são todos "extensão de quadril isolada" — mesma família.
const familiasBiomecanicas = {
  dobradica_de_quadril: ['STIFF', 'TERRA', 'BOM DIA'],
  agachamento: ['AGACHAMENTO', 'LEG PRESS', 'HACK'],
  unilateral_avanco: ['AFUNDO', 'PASSADA', 'BULGARO'],
  extensao_quadril_isolada: ['GLÚTEO', 'PONTE', 'COICE', 'ELEVAÇÃO PÉLVICA', 'HIP THRUST'],
  joelho_isolado: ['CADEIRA', 'EXTENSÃO', 'FLEXÃO'],
  abducao_adducao: ['ABDUÇÃO', 'ADUÇÃO']
};

function obterFamiliaBiomecanica(nomeExercicio){
  const upper = nomeExercicio.toUpperCase();
  const familias = Object.keys(familiasBiomecanicas);
  for(let i = 0; i < familias.length; i++){
    const palavras = familiasBiomecanicas[familias[i]];
    for(let j = 0; j < palavras.length; j++){
      if(upper.indexOf(palavras[j]) !== -1) return familias[i];
    }
  }
  return null; // sem família conhecida (ex: exercícios de superiores) — cai pra agrupamento por grupo muscular só
}

function selecionarExerciciosVariados(grupoInterno, nomeAluna, quantidade, indiceCiclo, padroesJaUsados, ambiente, evitarPliometrico){
  const pool = obterPoolRealDoGrupo(grupoInterno, ambiente, evitarPliometrico);
  if(pool.length === 0) return null;
  const seed = hashString(nomeAluna || '') + (indiceCiclo || 0) * 7;
  const selecionados = [];
  const usados = {};
  const padroesUsados = padroesJaUsados || {};
  for(let i = 0; i < quantidade; i++){
    let idx = (seed + i * 13) % pool.length;
    let tentativas = 0;
    let padrao = obterPalavraChaveMovimento(pool[idx].nome);
    // Nunca repete padrão de movimento no mesmo dia (ex: Stiff + Stiff com Halteres é o mesmo padrão)
    while((usados[idx] || padroesUsados[padrao]) && tentativas < pool.length){
      idx = (idx + 1) % pool.length;
      padrao = obterPalavraChaveMovimento(pool[idx].nome);
      tentativas++;
    }
    usados[idx] = true;
    padroesUsados[padrao] = true;
    selecionados.push(capitalizarNomeExercicio(pool[idx].nome));
  }
  return selecionados;
}

function gerarDiasInferiores(perfil, numDias, seriesTotais, diasCicloAnterior){
  // perfil.enfase = grupo prioritário (ex: 'Glúteo'), perfil.secundario = 2º grupo da pirâmide
  const enfase = perfil.enfase;
  const secundario = perfil.secundario;
  const reps = perfil.bloco === 'deload' ? 14 : (perfil.bloco === 'choque' ? 7 : (perfil.fase === 'Desempenho' ? 9 : 11));
  const metodo = perfil.bloco === 'choque' ? ('Cluster set (blocos de ' + sugerirBlocoCluster(perfil.indiceCiclo || 0) + ')') : 'Padrão';

  const bancoPorGrupo = {
    'Glúteo': ['Elevação Pélvica', 'Agachamento Hack, profundo', 'Abdução de quadril na polia'],
    'Posterior': ['Cadeira flexora', 'Extensão de quadril no banco romano', 'Flexora de Joelhos em Pé'],
    'Quadríceps': ['Leg Press 45', 'Agachamento Hack, profundo', 'Cadeira Extensora'],
    'Pernas': ['Leg Press 45', 'Cadeira flexora', 'Agachamento Hack, profundo']
  };
  function exerciciosDoGrupo(grupo, padroesJaUsadosNoDia){
    // ITEM 8/9 conectados: usa direcionamento técnico quando definido
    if(grupo === 'Glúteo' && perfil.direcionamentoGluteo && perfil.direcionamentoGluteo !== 'nenhum'){
      return sugerirExerciciosGluteo(perfil.direcionamentoGluteo).prioridade;
    }
    if(grupo === 'Quadríceps' && perfil.direcionamentoQuadriceps === 'distal'){
      return sugerirExerciciosQuadriceps('distal').prioridade;
    }
    const variados = selecionarExerciciosVariados(grupo, perfil.nomeAluna, 3, perfil.indiceCiclo, padroesJaUsadosNoDia, perfil.ambienteTreino, perfil.evitarPliometrico);
    if(variados) return variados;
    // Regra fixa da metodologia: nunca cruza ambiente, nem no fallback de emergência.
    // Se não achou exercício de Casa suficiente pro grupo, prefere ficar com menos exercícios
    // a preencher com exercício de academia.
    if(perfil.ambienteTreino === 'Casa') return [];
    return bancoPorGrupo[grupo] || bancoPorGrupo['Pernas'];
  }

  const dias = [];
  const posteriorJaContemplado = enfase === 'Posterior' || secundario === 'Posterior';

  for(let d = 0; d < numDias; d++){
    const ehDiaEnfase = d % 2 === 0; // dias pares = ênfase, ímpares = secundário (padrão validado com a Michele/Andriele)
    const grupoPrincipal = ehDiaEnfase ? enfase : (secundario || enfase);
    const grupoEstimulo = ehDiaEnfase ? secundario : enfase;
    const padroesUsadosNoDia = {}; // compartilhado entre ênfase e estímulo, pra nunca repetir padrão de movimento no mesmo dia

    let exPrincipais = exerciciosDoGrupo(grupoPrincipal, padroesUsadosNoDia).map(function(nome, posicao){
      // ITEM 11: rotação por família, só rotaciona a partir do 2º ciclo (indiceCiclo > 0)
      const nomeFinal = (perfil.indiceCiclo && perfil.indiceCiclo > 0) ? rotacionarExercicio(nome, false, perfil.indiceCiclo) : nome;
      return nomeFinal + ' · ' + (perfil.bloco === 'deload' ? 2 : (perfil.nivel === 'Avançado' ? 4 : 3)) + 'x' + reps;
    });
    // Fase de emagrecimento + nível qualifica + técnica aprovada: aumenta densidade combinando os 2 primeiros como bi-set
    if(perfil.bloco !== 'deload' && perfil.fase === 'Emagrecimento' && perfil.nivel !== 'Iniciante' && perfil.temTecnicaAprovada && exPrincipais.length >= 2){
      exPrincipais = ['Bi-set|||' + exPrincipais[0] + '|||' + exPrincipais[1]].concat(exPrincipais.slice(2));
    }
    const exEstimulo = grupoEstimulo ? [exerciciosDoGrupo(grupoEstimulo, padroesUsadosNoDia)[0] + ' · ' + (perfil.bloco === 'deload' ? 2 : 3) + 'x' + (reps + 2)] : [];
    const exPanturrilha = (perfil.frequencia <= 3) ? ['Panturrilha · 2x18'] : []; // baixa frequência: entra em todo dia, volume baixo
    dias.push({
      foco: 'Inferiores ' + String.fromCharCode(65 + d) + ' · ' + grupoPrincipal + (grupoEstimulo ? ' + estímulo ' + grupoEstimulo : ''),
      ex: exPrincipais.concat(exEstimulo).concat(exPanturrilha),
      metodo: metodo
    });
  }

  // Regra de volume mínimo de posteriores: se a pirâmide da aluna não tem Posterior como ênfase
  // nem secundário, ele nunca apareceria em nenhum dia. Garante pelo menos 1 exercício no último
  // dia de inferiores, com volume moderado (nem o volume de ênfase, nem tão baixo quanto panturrilha).
  if(!posteriorJaContemplado && dias.length > 0){
    const ultimoDia = dias[dias.length - 1];
    const opcoesPosterior = exerciciosDoGrupo('Posterior', {});
    if(opcoesPosterior && opcoesPosterior[0]){
      const seriesMinimas = perfil.bloco === 'deload' ? 2 : 3;
      ultimoDia.ex.push(opcoesPosterior[0] + ' · ' + seriesMinimas + 'x' + (reps + 2) + ' (volume mínimo de posteriores, garantido pela metodologia)');
    }
  }

  return dias;
}

function gerarDiasSuperiores(perfil, numDias){
  const reps = perfil.bloco === 'deload' ? 14 : (perfil.bloco === 'choque' ? 7 : (perfil.fase === 'Desempenho' ? 9 : 11));
  const seriesCostas = perfil.bloco === 'deload' ? 2 : (perfil.nivel === 'Avançado' ? 3 : 2);
  const inclureGluteoMedio = perfil.enfase === 'Glúteo';
  const dias = [];

  // Ordem composto-antes-de-isolado (item 3): Costas (puxada, composto) → Ombro/Peito (composto/misto) → Bíceps/Tríceps (isolado)
  for(let d = 0; d < numDias; d++){
    const padroesUsadosNoDia = {};

    // Costas: variedade real do banco (51 opções), sempre protegida com 2-3 exercícios, nunca menos
    const candidatosCostas = selecionarExerciciosVariados('Costas', (perfil.nomeAluna || '') + '_costas', 3, perfil.indiceCiclo, null, perfil.ambienteTreino, perfil.evitarPliometrico);
    const nomesCostas = candidatosCostas || ['Remada Articular Aberta', 'Remada Articular Fechada', 'Crucifixo Inverso'];
    nomesCostas.forEach(function(n){ padroesUsadosNoDia[obterPalavraChaveMovimento(n.toUpperCase())] = true; });
    let ex = [
      nomesCostas[0] + ' · ' + seriesCostas + 'x' + reps,
      nomesCostas[1] + ' · ' + seriesCostas + 'x' + reps
    ];
    if((perfil.indiceCiclo || 0) % 2 === 1 && nomesCostas[2]){
      ex.push(nomesCostas[2] + ' · ' + Math.max(2, seriesCostas - 1) + 'x' + reps);
    }

    // Ombro/Peito/Bíceps/Tríceps reais, alternando qual par recebe ênfase a cada dia (garante ≥2x/semana pra cada, em 3+ dias)
    const gruposSecundarios = (d % 2 === 0) ? ['Ombro', 'Peito'] : ['Bíceps', 'Tríceps'];
    gruposSecundarios.forEach(function(grupo){
      const candidato = selecionarExerciciosVariados(grupo, (perfil.nomeAluna || '') + '_' + grupo, 1, perfil.indiceCiclo, null, perfil.ambienteTreino, perfil.evitarPliometrico);
      if(candidato && candidato[0]){
        ex.push(candidato[0] + ' · ' + (perfil.nivel === 'Avançado' ? 3 : 2) + 'x' + reps);
      }
    });

    ex.push('Prancha ventral · 3x30s'); // isométrico de core, sem equipamento, funciona em qualquer ambiente
    if(inclureGluteoMedio && perfil.ambienteTreino !== 'Casa'){
      ex.push(sugerirReforcoCruzado('Abdução de quadril na polia'));
    } else if(inclureGluteoMedio){
      ex.push('Abdução de quadril elástico · 3x15'); // versão sem polia, pra treino em casa
    }
    if(perfil.frequencia <= 3 || d % 2 === 0 || numDias <= 2){
      ex.push('Panturrilha em pé · 3x15');
    }
    dias.push({ foco: 'Superiores ' + String.fromCharCode(65 + d) + ' · Costas, ' + gruposSecundarios.join('/') + ' e Abdômen', ex: ex });
  }
  return dias;
}

function gerarTreinoSemanal(perfil){
  // perfil: { nivel, enfase, secundario, frequencia, bloco, tempoDisponivel, indiceCiclo, semanaAtual, pausouRetomou }
  const numInferiores = Math.ceil(perfil.frequencia / 2);
  const numSuperiores = perfil.frequencia - numInferiores;

  const seriesTotais = calcularTetoVolume(perfil.nivel, perfil.semanaAtual || 1, !!perfil.pausouRetomou).teto *
    (perfil.bloco === 'deload' ? 0.5 : (perfil.bloco === 'choque' ? 0.82 : (perfil.volumeReduzidoPorFadiga ? 0.85 : 1))) *
    (perfil.estrategiaVolumeMultiplicador || 1);

  const diasInf = gerarDiasInferiores(perfil, numInferiores, seriesTotais);
  const diasSup = gerarDiasSuperiores(perfil, numSuperiores);

  const semana = [];
  let i = 0, s = 0;
  while(i < diasInf.length || s < diasSup.length){
    if(i < diasInf.length){ semana.push(diasInf[i]); i++; }
    if(s < diasSup.length){ semana.push(diasSup[s]); s++; }
  }

  function calcularDuracaoDia(dia){
    let total = 0;
    dia.ex.forEach(function(linha){
      const partes = linha.split(' · ');
      const seriesMatch = (partes[1] || '').match(/^(\d+)x/);
      const repsMatch = (partes[1] || '').match(/x(\d+)/);
      const series = seriesMatch ? parseInt(seriesMatch[1], 10) : 3;
      const reps = repsMatch ? parseInt(repsMatch[1], 10) : 12;
      const descanso = calcularDescansoPorReps(reps);
      const unilateral = ehExercicioUnilateral(partes[0]);
      if(descanso) total += estimarDuracaoExercicio(series, descanso, unilateral);
    });
    return total;
  }

  function bumpSeries(linha, jaTemExercicioNoTeto){
    if(linha.indexOf('|||') !== -1) return { linha: linha, chegouNoTeto: false }; // não mexe em bi-set
    if(linha.toUpperCase().indexOf('PANTURRILHA') !== -1) return { linha: linha, chegouNoTeto: false }; // panturrilha mantém volume baixo de propósito
    if(linha.toUpperCase().indexOf('PRANCHA') !== -1) return { linha: linha, chegouNoTeto: false }; // isométrico, não segue a mesma lógica de teto de peso
    if(linha.indexOf('carga baixa') !== -1) return { linha: linha, chegouNoTeto: false }; // reforço cruzado, é carga baixa de propósito, não conta pro teto
    const match = linha.match(/^([^·]+·\s*)(\d+)x/);
    if(!match) return { linha: linha, chegouNoTeto: false };
    const seriesAtuais = parseInt(match[2], 10);
    const tetoPorNivel = { 'Iniciante': 3, 'Intermediário': 4, 'Avançado': 5 };
    const tetoNivel = perfil.bloco === 'deload' ? 2 : (tetoPorNivel[perfil.nivel] || 4);
    // Regra real: no máximo 1 exercício do dia pode chegar no teto do nível, não importa qual, nunca 2 ou mais
    const tetoPermitido = jaTemExercicioNoTeto.valor ? tetoNivel - 1 : tetoNivel;
    if(seriesAtuais >= tetoPermitido) return { linha: linha, chegouNoTeto: seriesAtuais >= tetoNivel };
    const novaLinha = linha.replace(/^([^·]+·\s*)(\d+)x/, function(match, prefixo, num){
      return prefixo + (parseInt(num, 10) + 1) + 'x';
    });
    const novasSeries = seriesAtuais + 1;
    return { linha: novaLinha, chegouNoTeto: novasSeries >= tetoNivel };
  }

  semana.forEach(function(dia){
    // ITEM: reforço automático quando sobra tempo real (regra: usar quase 100% do tempo disponível)
    // No deload, a sessão mais curta é intencional, nunca reforça pra preencher tempo
    if(perfil.tempoDisponivel && perfil.bloco !== 'deload' && !perfil.volumeReduzidoPorFadiga){
      let tentativasReforco = 0;
      const jaTemExercicioNoTeto = { valor: false };
      while(calcularDuracaoDia(dia) < perfil.tempoDisponivel * 0.95 && tentativasReforco < 15){
        let algumMudou = false;
        dia.ex = dia.ex.map(function(linha){
          const resultado = bumpSeries(linha, jaTemExercicioNoTeto);
          if(resultado.linha !== linha) algumMudou = true;
          if(resultado.chegouNoTeto) jaTemExercicioNoTeto.valor = true;
          return resultado.linha;
        });
        tentativasReforco++;
        if(!algumMudou) break; // todo mundo já travou no teto permitido, parar de tentar
      }

      // Ainda sobra tempo real, mesmo respeitando o teto de série? Adiciona MAIS EXERCÍCIOS (não mais série),
      // priorizando a ênfase real da pirâmide, até usar quase 100% do tempo disponível
      const gruposSuperioresPossiveis = ['Ombro', 'Peito', 'Bíceps', 'Tríceps', 'Costas'];
      const ehInferior = (perfil.enfase && dia.foco.indexOf(perfil.enfase) !== -1) || (perfil.secundario && dia.foco.indexOf(perfil.secundario) !== -1);
      const ehSuperior = !ehInferior && gruposSuperioresPossiveis.some(function(g){ return dia.foco.indexOf(g) !== -1; });
      if(ehInferior || ehSuperior){
        const grupoDoDia = ehInferior
          ? (dia.foco.indexOf(perfil.enfase) !== -1 ? perfil.enfase : perfil.secundario)
          : gruposSuperioresPossiveis.find(function(g){ return dia.foco.indexOf(g) !== -1; }) || 'Costas';
        let tentativasAdicionar = 0;
        const tetoExerciciosPorNivel = { 'Iniciante': 5, 'Intermediário': 6, 'Avançado': 7 };
        const tetoExerciciosDoDia = tetoExerciciosPorNivel[perfil.nivel] || 6;
        while(calcularDuracaoDia(dia) < perfil.tempoDisponivel * 0.9 && tentativasAdicionar < 8 && dia.ex.length < tetoExerciciosDoDia){
          const nomesJaNoDia = dia.ex.map(function(l){ return l.split(' · ')[0].toUpperCase(); });
          const padroesJaNoDia = {};
          nomesJaNoDia.forEach(function(n){ padroesJaNoDia[obterPalavraChaveMovimento(n)] = true; });
          const candidatos = selecionarExerciciosVariados(grupoDoDia, (perfil.nomeAluna || '') + '_extra' + tentativasAdicionar, 8, perfil.indiceCiclo, null, perfil.ambienteTreino, perfil.evitarPliometrico);
          let escolhido = null;
          if(candidatos){
            escolhido = candidatos.find(function(nome){
              return nomesJaNoDia.indexOf(nome.toUpperCase()) === -1 && !padroesJaNoDia[obterPalavraChaveMovimento(nome)];
            });
          }
          if(!escolhido){
            // A amostra embaralhada não tinha opção válida — antes de desistir, busca direto no banco inteiro (não só na amostra)
            const todoOBanco = exerciciosBanco.filter(function(e){
              if((e.grupo || e.categoria) !== grupoDoDia) return false;
              if(perfil.evitarPliometrico && ehExercicioPliometrico(e)) return false;
              return perfil.ambienteTreino === 'Casa' ? exercicioServeParaCasa(e) : ((e.ambiente || 'Academia') === 'Academia' && !ehExercicioElastico(e));
            });
            const candidatoDeResgate = todoOBanco.find(function(e){
              return nomesJaNoDia.indexOf(e.nome.toUpperCase()) === -1 && !padroesJaNoDia[obterPalavraChaveMovimento(e.nome)];
            });
            if(candidatoDeResgate) escolhido = candidatoDeResgate.nome;
          }
          if(!escolhido) break; // sem candidato novo e distinto, para de tentar
          const tetoPorNivel = { 'Iniciante': 3, 'Intermediário': 4, 'Avançado': 5 };
          const seriesNovoEx = (tetoPorNivel[perfil.nivel] || 4) - 1; // nunca reivindica o teto máximo, esse já está ocupado
          dia.ex.push(escolhido + ' · ' + seriesNovoEx + 'x' + (perfil.bloco === 'choque' ? 7 : 11) + ' (reforço da ênfase, tempo disponível permitia mais)');
          tentativasAdicionar++;
        }
      }
    }

    // ITEM: corte automático se exceder o tempo disponível
    // Protege: 2 primeiros exercícios (Costas em superiores / ênfase principal em inferiores) e a panturrilha, corta do fim pra início
    const minimoProtegido = 2;
    if(perfil.tempoDisponivel){
      while(calcularDuracaoDia(dia) > perfil.tempoDisponivel && dia.ex.length > minimoProtegido){
        // Acha o último exercício removível (de trás pra frente), pulando a panturrilha de propósito
        let idxRemover = -1;
        for(let k = dia.ex.length - 1; k >= minimoProtegido; k--){
          if(dia.ex[k].toUpperCase().indexOf('PANTURRILHA') === -1){ idxRemover = k; break; }
        }
        if(idxRemover === -1) break; // só sobrou panturrilha e os protegidos, para de cortar
        dia.ex.splice(idxRemover, 1);
        dia.cortadoPorTempo = true;
      }
    }
    const duracaoFinal = calcularDuracaoDia(dia);
    dia.duracaoEstimadaMin = Math.round(duracaoFinal);
    dia.excedeTempo = perfil.tempoDisponivel ? duracaoFinal > perfil.tempoDisponivel : false;
  });

  // Renomeia sequencialmente na ordem da semana: Treino A, Treino B, Treino C... sem distinguir Inferiores/Superiores no nome
  let letraTreino = 0;
  semana.forEach(function(dia){
    const descricao = dia.foco.indexOf(' · ') !== -1 ? dia.foco.split(' · ').slice(1).join(' · ') : dia.foco;
    dia.foco = 'Treino ' + String.fromCharCode(65 + letraTreino) + ' · ' + descricao;
    letraTreino++;
  });

  // Progressão de reps por posição no dia: 8, 10, 12, 15 (do exercício principal pro final), só no bloco base
  if(perfil.bloco !== 'choque' && perfil.bloco !== 'deload'){
    const sequenciaReps = [8, 10, 12, 15];
    semana.forEach(function(dia){
      let posicao = 0;
      dia.ex = dia.ex.map(function(linha){
        if(linha.indexOf('|||') !== -1) return linha; // bi-set mantém a própria lógica
        const upper = linha.toUpperCase();
        if(upper.indexOf('PANTURRILHA') !== -1 || upper.indexOf('PRANCHA') !== -1 || linha.indexOf('carga baixa') !== -1) return linha;
        const match = linha.match(/^([^·]+·\s*\d+x)(\d+)(.*)$/);
        if(!match) return linha;
        const novoReps = sequenciaReps[Math.min(posicao, sequenciaReps.length - 1)];
        posicao++;
        return match[1] + novoReps + match[3];
      });
    });
  }

  return semana;
}

/* ===== VALIDADOR DE PRESCRIÇÃO, investigação rigorosa automática ===== */

function validarPrescricao(perfil, semana){
  const checklist = [];

  function contemPalavra(nomeEx, lista){
    const upper = nomeEx.toUpperCase();
    return lista.some(function(p){ return upper.indexOf(p) !== -1; });
  }

  // 1. Nível determinado
  checklist.push({ item: 'Nível determinado', ok: !!perfil.nivel, detalhe: perfil.nivel || 'faltando' });

  // 2. Fase / override de IMC decidido explicitamente (não deixado em aberto)
  checklist.push({ item: 'Fase/override de IMC decidido (não pendente)', ok: perfil.faseDecidida === true, detalhe: perfil.faseDecidida ? 'decidido' : 'PENDENTE, decidir antes de fechar' });

  // 3. Panturrilha com frequência adequada (todo dia se baixa frequência, senão 2x)
  const diasComPanturrilha = semana.filter(function(d){ return d.ex.some(function(e){ return contemPalavra(e, ['PANTURRILHA']); }); }).length;
  const minimoPanturrilha = perfil.frequencia <= 3 ? semana.length : 2;
  checklist.push({ item: 'Panturrilha com frequência adequada', ok: diasComPanturrilha >= minimoPanturrilha, detalhe: diasComPanturrilha + ' de ' + semana.length + ' dias (mínimo ' + minimoPanturrilha + ')' });

  // 4. Ênfase presente em todo dia de inferiores
  const diasInferiores = semana.filter(function(d){ return d.foco.indexOf('Inferiores') !== -1; });
  const enfaseEmTodos = diasInferiores.length === 0 || diasInferiores.every(function(d){ return d.foco.indexOf(perfil.enfase) !== -1 || d.ex.some(function(e){ return contemPalavra(e, [perfil.enfase.toUpperCase()]); }); });
  checklist.push({ item: 'Ênfase presente em todo dia de inferiores', ok: enfaseEmTodos, detalhe: diasInferiores.length + ' dias de inferiores' });

  // 5. Costas protegida nos dias de superiores
  const diasSuperiores = semana.filter(function(d){ return d.foco.indexOf('Superiores') !== -1; });
  const costasOk = diasSuperiores.every(function(d){
    const count = d.ex.filter(function(e){ return contemPalavra(e, ['REMADA','PUXADA','CRUCIFIXO INVERSO','DESENVOLVIMENTO']); }).length;
    return count >= 2;
  });
  checklist.push({ item: 'Costas protegida (mín. 2 exercícios/dia)', ok: costasOk });

  // 6. Glúteo médio em superiores quando ênfase é Glúteo
  if(perfil.enfase === 'Glúteo'){
    const temAbducao = diasSuperiores.length === 0 || diasSuperiores.every(function(d){ return d.ex.some(function(e){ return contemPalavra(e, ['ABDUÇÃO']); }); });
    checklist.push({ item: 'Glúteo médio incluído em superiores (ênfase é Glúteo)', ok: temAbducao });
  }

  // 7. Duração usa o tempo disponível de forma razoável (nunca excede; pode ficar mais curta quando o teto de exercícios por nível for atingido antes,
  // ou em semana de deload, onde sessão curta é intencional por metodologia — nenhum dos dois é bug)
  if(perfil.tempoDisponivel && perfil.bloco !== 'deload'){
    const duracaoOk = semana.every(function(d){
      const dur = d.duracaoEstimadaMin != null ? d.duracaoEstimadaMin : 0;
      return dur <= perfil.tempoDisponivel && dur >= perfil.tempoDisponivel * 0.3;
    });
    checklist.push({ item: 'Duração dentro do tempo disponível, sem exceder', ok: duracaoOk, detalhe: semana.map(function(d){ return (d.duracaoEstimadaMin != null ? d.duracaoEstimadaMin : '?') + 'min'; }).join(', ') + ' de ' + perfil.tempoDisponivel + 'min' });
  } else if(perfil.bloco === 'deload'){
    checklist.push({ item: 'Duração dentro do tempo disponível, sem exceder', ok: true, detalhe: 'Semana de deload, sessão curta é intencional' });
  }

  // 8. Correção postural incluída se houver desvio confirmado
  if(perfil.desviosPosturais && perfil.desviosPosturais.length > 0){
    checklist.push({ item: 'Bloco de correção postural aplicado', ok: perfil.blocoPosturalAplicado === true, detalhe: perfil.blocoPosturalAplicado ? 'aplicado' : 'PENDENTE, desvio confirmado mas bloco não incluído' });
  }

  // 9. Queixa de dor tratada como sugestão, nunca substituição silenciosa
  if(perfil.queixaDor){
    checklist.push({ item: 'Queixa de dor sinalizada formalmente (não substituição silenciosa)', ok: perfil.queixaSinalizadaFormalmente === true, detalhe: perfil.queixaSinalizadaFormalmente ? 'sinalizada' : 'PENDENTE, confirmar com o personal' });
  }

  const aprovado = checklist.every(function(c){ return c.ok; });
  return { aprovado: aprovado, checklist: checklist };
}

function renderChecklistPrescricao(resultado){
  let html = '<p class="section-label" style="margin-top:10px;">Validação automática da prescrição</p>';
  resultado.checklist.forEach(function(c){
    html += '<div class="list-item"><span><i class="ti ti-' + (c.ok ? 'circle-check' : 'circle-x') + '" style="font-size:13px;color:' + (c.ok ? 'var(--gold-soft)' : '#E2A33D') + ';vertical-align:-2px;margin-right:6px;"></i>' + c.item + '</span>' + (c.detalhe ? '<span class="tag">' + c.detalhe + '</span>' : '') + '</div>';
  });
  html += '<div class="' + (resultado.aprovado ? 'insight' : 'info-box') + '" style="margin-top:8px;"><p' + (resultado.aprovado ? '' : ' class="txt"') + '>' + (resultado.aprovado ? '✅ Prescrição aprovada, todos os critérios da metodologia foram cumpridos.' : '⚠️ Ainda há pendências, revise os itens marcados acima antes de fechar com a aluna.') + '</p></div>';
  return html;
}

function calcularVolumePorCategoria(a){
  if(!a.treinoAtual) return { totais: {}, duracaoTotalMin: 0 };
  const totais = {};
  let duracaoTotalMin = 0;
  a.treinoAtual.dias.forEach(function(d){
    d.ex.forEach(function(linhaOriginal){
      const linhas = linhaOriginal.indexOf('|||') !== -1
        ? linhaOriginal.split('|||').slice(1).map(function(l){ return l.trim(); })
        : [linhaOriginal];
      linhas.forEach(function(linha){
        const partes = linha.split(' · ');
        const nomeEx = partes[0];
        const seriesMatch = (partes[1] || '').match(/^(\d+)x/);
        const series = seriesMatch ? parseInt(seriesMatch[1], 10) : 0;
        const repsMatch = (partes[1] || '').match(/x(\d+)/);
        const reps = repsMatch ? parseInt(repsMatch[1], 10) : null;
        const exBanco = buscarExercicioNoBanco(nomeEx);
        const categoria = exBanco ? exBanco.categoria : 'Outro';
        const unilateral = ehExercicioUnilateral(nomeEx);
        const descanso = calcularDescansoPorReps(reps);

        if(contaVolumeDuplo(nomeEx)){
          totais['Quadríceps'] = (totais['Quadríceps'] || 0) + series;
          totais['Glúteos'] = (totais['Glúteos'] || 0) + series;
        } else {
          totais[categoria] = (totais[categoria] || 0) + series;
        }

        if(descanso){
          duracaoTotalMin += estimarDuracaoExercicio(series, descanso, unilateral);
        }
      });
    });
  });
  return { totais: totais, duracaoTotalMin: Math.round(duracaoTotalMin) };
}

function renderProgressao(a){
  const prog = getProgressoAluna(a.nome);
  const exercicios = extrairExercicios(a);
  if(exercicios.length === 0) return '';

  let opcoes = exercicios.map(function(n){ return '<option value="' + n + '">' + n + '</option>'; }).join('');

  let logHtml = '';
  Object.keys(prog.historico).forEach(function(nomeEx){
    const registros = prog.historico[nomeEx];
    if(registros.length === 0) return;
    logHtml += '<p style="font-size:12px;font-weight:600;color:var(--text-dim);margin:10px 0 6px;">' + nomeEx + '</p>';
    registros.forEach(function(r){
      logHtml += '<div class="list-item"><span>Semana ' + r.semana + ' · ' + r.carga + 'kg × ' + r.reps + ' reps</span><span class="tag">' + r.sugestao.texto + ' → ' + r.sugestao.valor + 'kg</span></div>';
    });
  });
  if(!logHtml){
    logHtml = '<div class="info-box"><p class="txt">Nenhuma sessão registrada ainda, a aluna registra pelo próprio treino, ou simule aqui.</p></div>';
  }

  const totalDias = totalDiasDeTreino();
  const concluidosSemana = (prog.diasConcluidos[prog.semana] || []).length;

  let reavaliacaoHtml = '';
  if(prog.semana >= 6){
    reavaliacaoHtml =
      '<div class="insight" style="margin-top:14px;"><p><b>Reavaliação de ciclo (semana ' + prog.semana + ')</b><br>' +
      'Hora de revisar: nível ainda condiz com a execução observada? Algum exercício estabilizou e pode evoluir de complexidade? Vale reavaliar peso/medidas para confirmar se a Fase 1 (emagrecimento) ainda é prioridade ou se já pode avançar para a ênfase estética declarada.</p></div>';
  }

  const volumeResultado = calcularVolumePorCategoria(a);
  const volumePorCategoria = volumeResultado.totais;
  const infoTeto = calcularTetoVolume(a.nivel, prog.semana, !!a.pausouRetomou);
  let volumeHtml = '<p class="section-label" style="margin-top:22px;">Volume total da semana (visível só pra você)</p><div class="info-box">';
  Object.keys(volumePorCategoria).forEach(function(cat){
    volumeHtml += '<p class="txt">' + cat + ': ' + volumePorCategoria[cat] + ' séries</p>';
  });
  volumeHtml += '<p class="txt" style="color:var(--gold-soft);margin:6px 0;">⏱ Duração total estimada da semana: ~' + volumeResultado.duracaoTotalMin + ' min</p>';
  volumeHtml += '<p class="txt" style="color:var(--text-faint);font-size:12px;margin-bottom:0;">Teto de volume atual: ' + infoTeto.teto + ' séries/semana por ênfase' + (infoTeto.emTransicao ? ' · Volume de transição (retomada), libera pleno na semana ' + (SEMANAS_TRANSICAO + 1) : ' (' + a.nivel + ')') + '</p></div>';

  return '<p class="section-label" style="margin-top:22px;">Progressão de carga</p>' +
    '<div class="list-item" style="margin-bottom:10px;"><span>Semana atual do ciclo: ' + prog.semana + ' / 6</span></div>' +
    '<div class="list-item" style="margin-bottom:10px;"><span>Treinos concluídos essa semana: ' + concluidosSemana + ' / ' + totalDias + '</span></div>' +
    logHtml +
    reavaliacaoHtml +
    volumeHtml +
    '<button class="btn-gold" style="background:var(--card-2);color:var(--gold-soft);border:1px solid var(--border);margin-top:14px;" onclick="exportarRelatorioEvolucao(\'' + a.nome.replace(/'/g,"\\'") + '\')"><i class="ti ti-file-export" style="font-size:13px;vertical-align:-2px;margin-right:6px;"></i>Exportar relatório de evolução</button>' +
    '<p style="font-size:11px;color:var(--text-faint);text-align:center;margin-top:16px;cursor:pointer;" onclick="resetarDadosTeste()">Resetar dados de teste (antes do lançamento)</p>';
}

/* ===== FEEDBACK SOBRE O GERADOR, coletado organizado pra virar regra numa próxima conversa ===== */
function carregarFeedbackGerador(){
  try {
    const raw = localStorage.getItem('musa_feedback_gerador');
    return raw ? JSON.parse(raw) : [];
  } catch(e){ return []; }
}
function salvarListaFeedbackGerador(lista){
  try { localStorage.setItem('musa_feedback_gerador', JSON.stringify(lista)); } catch(e){}
}

async function salvarFeedbackGerador(nomeAluna){
  const textoEl = document.getElementById('feedback-gerador-texto');
  const texto = textoEl.value.trim();
  if(!texto) return;
  const registro = { data: new Date().toLocaleDateString('pt-BR'), aluna: nomeAluna, texto: texto };

  const lista = carregarFeedbackGerador();
  lista.push(registro);
  salvarListaFeedbackGerador(lista);
  textoEl.value = '';
  renderListaFeedbackGerador(nomeAluna);

  // Salva de verdade no banco também, pra não depender só do navegador local
  const chave = 'feedback_' + Date.now();
  if(supabaseClient){
    try {
      const { error } = await supabaseClient.from('catalogo_personal').upsert({
        tipo: 'feedback_gerador', chave: chave, dados: registro, updated_at: new Date().toISOString()
      });
      mostrarConfirmacaoSalvamento(!error, error ? 'Feedback salvo só localmente por enquanto: ' + error.message : 'Feedback anotado e salvo de verdade.');
    } catch(erroDeRede){
      mostrarConfirmacaoSalvamento(false, 'Sem conexão agora — feedback ficou salvo só nesse navegador, tenta de novo com internet.');
    }
  } else {
    mostrarConfirmacaoSalvamento(false, 'Sem conexão com o Supabase — feedback ficou salvo só nesse navegador.');
  }
}

function renderListaFeedbackGerador(nomeAluna){
  const container = document.getElementById('lista-feedback-gerador');
  if(!container) return;
  const listaCompleta = carregarFeedbackGerador();
  const lista = nomeAluna ? listaCompleta.filter(function(f){ return f.aluna === nomeAluna; }) : listaCompleta;
  if(lista.length === 0){ container.innerHTML = ''; return; }
  let html = '<p class="lbl" style="margin-top:14px;">Feedbacks guardados dessa aluna (' + lista.length + ')</p>';
  lista.slice().reverse().forEach(function(f){
    html += '<div class="info-box" style="margin-bottom:8px;"><p class="txt" style="margin-bottom:2px;">' + f.texto + '</p><p style="font-size:11px;color:var(--text-faint);margin:0;">' + f.aluna + ' · ' + f.data + '</p></div>';
  });
  html += '<div style="display:flex;gap:8px;">' +
    '<button class="btn-gold" style="background:var(--card-2);color:var(--gold-soft);border:1px solid var(--border);font-size:12px;width:auto;flex:1;" onclick="copiarFeedbackGerador(\'' + (nomeAluna || '').replace(/'/g,"\\'") + '\')">Copiar tudo</button>' +
    '<button class="btn-gold" style="background:var(--card-2);color:var(--gold-soft);border:1px solid var(--border);font-size:12px;width:auto;flex:1;" onclick="baixarFeedbackGerador(\'' + (nomeAluna || '').replace(/'/g,"\\'") + '\')"><i class="ti ti-download" style="font-size:12px;vertical-align:-2px;margin-right:4px;"></i>Baixar arquivo</button>' +
  '</div>';
  container.innerHTML = html;
}

function copiarFeedbackGerador(nomeAluna){
  const listaCompleta = carregarFeedbackGerador();
  const lista = nomeAluna ? listaCompleta.filter(function(f){ return f.aluna === nomeAluna; }) : listaCompleta;
  const texto = lista.map(function(f){ return '[' + f.data + ' · ' + f.aluna + '] ' + f.texto; }).join('\n');
  if(navigator.clipboard) navigator.clipboard.writeText(texto);
}

function baixarFeedbackGerador(nomeAluna){
  const listaCompleta = carregarFeedbackGerador();
  const lista = nomeAluna ? listaCompleta.filter(function(f){ return f.aluna === nomeAluna; }) : listaCompleta;
  const texto = lista.map(function(f){ return '[' + f.data + ' · ' + f.aluna + '] ' + f.texto; }).join('\n');
  const blob = new Blob([texto], { type: 'text/plain;charset=utf-8' });
  const url = URL.createObjectURL(blob);
  const link = document.createElement('a');
  link.href = url;
  link.download = 'musa_feedbacks_' + (nomeAluna ? nomeAluna.replace(/[^a-zA-Z0-9]/g,'_') + '_' : '') + new Date().toISOString().slice(0,10) + '.txt';
  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
  URL.revokeObjectURL(url);
}

// ===== MOTOR 2: CLASSIFICAÇÃO AUTOMÁTICA =====
// Agrega tudo que já calculávamos separado (teto de volume, fadiga, idade, técnica) num diagnóstico único,
// que alimenta os motores seguintes (Periodização, Estratégia, Métodos).
function classificarAluna(a){
  const prog = getProgressoAluna(a.nome);
  const fadiga = avaliarSinaisDeFadiga(a);
  const ajusteIdade = ajusteRecuperacaoPorIdade(a.idade || null);
  const tecnicasAprovadas = a.tecnicaAprovada ? Object.keys(a.tecnicaAprovada).filter(function(k){ return a.tecnicaAprovada[k] === 'aprovado'; }).length : 0;
  const stats = calcularEstatisticasAluna(a.nome);
  const piramideInfo = extrairEnfaseSecundaria(a.piramide);
  const freqMatch = (a.freq || '').match(/\d+/);
  const freqDesejada = freqMatch ? parseInt(freqMatch[0], 10) : 3;

  let capacidadeRecuperacao = 'normal';
  if(ajusteIdade.ajuste < 0 || fadiga.nivel === 'alto') capacidadeRecuperacao = 'reduzida';
  else if(fadiga.nivel === 'baixo' && prog.semana > 4) capacidadeRecuperacao = 'boa';

  let experienciaTecnica = 'iniciante';
  if(a.nivel === 'Avançado' || tecnicasAprovadas >= 3) experienciaTecnica = 'avançada';
  else if(a.nivel === 'Intermediário' || tecnicasAprovadas >= 1) experienciaTecnica = 'intermediária';

  const frequenciaIdeal = capacidadeRecuperacao === 'reduzida' ? Math.max(2, freqDesejada - 1) : freqDesejada;

  return {
    nivel: a.nivel || 'Iniciante',
    volumeToleradoSeries: calcularTetoVolume(a.nivel || 'Iniciante', prog.semana, false).teto,
    capacidadeRecuperacao: capacidadeRecuperacao,
    experienciaTecnica: experienciaTecnica,
    frequenciaDesejada: freqDesejada,
    frequenciaIdeal: frequenciaIdeal,
    grupoPrioritario: piramideInfo.enfase,
    grupoSecundario: piramideInfo.secundario,
    fadiga: fadiga,
    constanciaPct: stats.temDados ? Math.round((stats.totalConcluidos / stats.totalPlanejado) * 100) : null
  };
}

function determinarFaseAluna(a){
  if(a.pesoAtual && a.altura){
    const imc = a.pesoAtual / (a.altura * a.altura);
    if(imc >= 25) return 'Emagrecimento';
  }
  const objetivoTexto = (a.objetivo || '').toUpperCase();
  if(objetivoTexto.indexOf('EMAGREC') !== -1 || objetivoTexto.indexOf('PERDA DE PESO') !== -1 || objetivoTexto.indexOf('PERDER PESO') !== -1){
    return 'Emagrecimento';
  }
  if(objetivoTexto.indexOf('DESEMPENHO') !== -1 || objetivoTexto.indexOf('PERFORMANCE') !== -1 || objetivoTexto.indexOf('FORÇA') !== -1 || objetivoTexto.indexOf('FORCA') !== -1){
    return 'Desempenho';
  }
  return 'Estética/Geral';
}

function temTecnicaAprovada(a){
  if(!a.tecnicaAprovada) return false;
  return Object.keys(a.tecnicaAprovada).some(function(k){ return a.tecnicaAprovada[k] === 'aprovado'; });
}

function extrairEnfaseSecundaria(piramideTexto){
  const upper = (piramideTexto || '').toUpperCase();
  const mapaKeywords = [
    { termos: ['GLÚTEO', 'GLUTEO', 'BUMBUM', 'BUNDA'], grupo: 'Glúteo' },
    { termos: ['QUADRÍCEPS', 'QUADRICEPS', 'COXA', 'PERNA'], grupo: 'Quadríceps' },
    { termos: ['POSTERIOR'], grupo: 'Posterior' }
  ];
  const encontrados = [];
  const termosEncontrados = [];
  // Varre o texto caractere a caractere, registrando a posição de cada termo encontrado, na ordem em que aparecem
  const ocorrencias = [];
  mapaKeywords.forEach(function(item){
    item.termos.forEach(function(termo){
      const pos = upper.indexOf(termo);
      if(pos !== -1) ocorrencias.push({ pos: pos, grupo: item.grupo, termo: termo });
    });
  });
  ocorrencias.sort(function(a, b){ return a.pos - b.pos; });
  ocorrencias.forEach(function(o){
    if(encontrados.indexOf(o.grupo) === -1){ encontrados.push(o.grupo); termosEncontrados.push(o.termo); }
  });
  // "Perna" é um termo genérico, sem especificar quadríceps ou posterior, então balanceia os dois em vez de cair no padrão de Glúteo
  if(termosEncontrados[0] === 'PERNA'){
    return { enfase: 'Quadríceps', secundario: 'Posterior' };
  }
  return {
    enfase: encontrados[0] || 'Glúteo',
    secundario: encontrados[1] || (encontrados[0] === 'Quadríceps' ? 'Glúteo' : 'Posterior')
  };
}

function calcularTempoRealDeMusculacao(a, blocoInfo){
  const tetoPorNivel = { 'Iniciante': 60, 'Intermediário': 60, 'Avançado': 70 };
  const teto = tetoPorNivel[a.nivel] || 60;
  const prog = getProgressoAluna(a.nome);
  // Retomando após pausa, ou ainda nas primeiras semanas: começa conservador (45min), sobe conforme o feedback for vindo
  const emTransicaoOuInicio = prog.semana <= 4;
  if(emTransicaoOuInicio) return Math.min(45, teto);
  return teto; // o tempo declarado pela aluna vira teto do que sobra pra cardio, nunca aumenta a musculação além disso
}

function construirPerfilAluna(a){
  const piramideInfo = extrairEnfaseSecundaria(a.piramide);
  const enfase = piramideInfo.enfase;
  const secundario = piramideInfo.secundario;
  const blocoInfo = calcularBlocoAtual(a);
  const freqMatch = (a.freq || '').match(/\d+/);
  const freqNum = freqMatch ? parseInt(freqMatch[0], 10) : 3;
  const prog = getProgressoAluna(a.nome);
  const semanaReal = prog.semana || 1;
  // Ciclo avança a cada bloco de periodização completo (4 semanas: base/base/choque/deload),
  // não a cada geração de treino — isso é o que garante variedade real de exercício conforme ela evolui,
  // em vez de gerar sempre a mesma seleção não importa quantas semanas ela já tenha treinado.
  const progressaoManual = a.progressaoManualForcada || 0;
  const cicloReal = Math.floor((semanaReal - 1) / 4) + progressaoManual;
  const estrategia = definirEstrategiaAtual(a);
  return {
    nivel: a.nivel || 'Iniciante',
    enfase: enfase,
    secundario: secundario,
    nomeAluna: a.nome,
    frequencia: freqNum,
    bloco: blocoInfo.blocoTecnico,
    volumeReduzidoPorFadiga: !!blocoInfo.volumeReduzido,
    tempoDisponivel: calcularTempoRealDeMusculacao(a, blocoInfo),
    indiceCiclo: cicloReal,
    semanaAtual: semanaReal,
    pausouRetomou: estrategia.nome === 'Retorno pós-pausa',
    faseDecidida: true,
    fase: determinarFaseAluna(a),
    temTecnicaAprovada: temTecnicaAprovada(a),
    estrategiaAtual: estrategia.nome,
    estrategiaMotivo: estrategia.motivo,
    // Cada progressão manual soma +8% de volume, além do que a estratégia já calculava —
    // é o "empurrão" que você decide dar, mesmo sem dado suficiente ainda
    // Risco de abandono (3 semanas seguidas com constância baixa) reduz volume mais um pouco,
    // pra facilitar ela voltar a conseguir cumprir — além da própria estratégia já ter caído pra "Manutenção"
    estrategiaVolumeMultiplicador: estrategia.volumeMultiplicador * (1 + progressaoManual * 0.08) * (detectarRiscoAbandonoPorConstancia(a.nome) ? 0.85 : 1),
    riscoAbandonoPorConstancia: detectarRiscoAbandonoPorConstancia(a.nome),
    ambienteTreino: a.ambienteTreino === 'Casa' ? 'Casa' : 'Academia',
    // Regra da metodologia: dor no joelho relatada (confirmada ou só no texto de restrições) tira pliométrico do treino
    evitarPliometrico: a.regiaoQueixa === 'Joelho' || (a.restricoes && a.restricoes.toLowerCase().indexOf('joelho') !== -1)
  };
}

function calcularSnapshotVolume(diasSemana){
  const totais = {};
  let duracaoTotal = 0;
  diasSemana.forEach(function(dia){
    (dia.ex || []).forEach(function(linhaOriginal){
      const linhas = linhaOriginal.indexOf('|||') !== -1 ? linhaOriginal.split('|||').slice(1).map(function(l){ return l.trim(); }) : [linhaOriginal];
      linhas.forEach(function(linha){
        const partes = linha.split(' · ');
        const nomeEx = partes[0];
        const seriesMatch = (partes[1] || '').match(/^(\d+)x/);
        const series = seriesMatch ? parseInt(seriesMatch[1], 10) : 0;
        const exBanco = buscarExercicioNoBanco(nomeEx);
        const categoria = exBanco ? (exBanco.grupo || exBanco.categoria) : 'Outro';
        if(contaVolumeDuplo(nomeEx)){
          totais['Quadríceps'] = (totais['Quadríceps'] || 0) + series;
          totais['Glúteos'] = (totais['Glúteos'] || 0) + series;
        } else {
          totais[categoria] = (totais[categoria] || 0) + series;
        }
      });
    });
    duracaoTotal += dia.duracaoEstimadaMin || 0;
  });
  return { totais: totais, duracaoTotal: Math.round(duracaoTotal) };
}

let treinoPreviewPendente = null;

function mostrarPreviewMudancaTreino(nomeAluna){
  const a = alunasPersonal.find(function(x){ return x.nome === nomeAluna; });
  if(!a) return;
  const area = document.getElementById('preview-mudanca-area');
  if(!area) return;

  const snapshotAntes = a.treinoAtual ? calcularSnapshotVolume(a.treinoAtual.dias) : { totais: {}, duracaoTotal: 0 };
  const perfil = construirPerfilAluna(a);
  const semanaPreview = gerarTreinoSemanal(perfil);
  const snapshotDepois = calcularSnapshotVolume(semanaPreview);
  const resultadoValidacao = validarPrescricao(perfil, semanaPreview);

  treinoPreviewPendente = { nomeAluna: nomeAluna, semana: semanaPreview };

  const categorias = Array.from(new Set(Object.keys(snapshotAntes.totais).concat(Object.keys(snapshotDepois.totais))));
  let comparativoHtml = '<div class="info-box">';
  if(categorias.length === 0){
    comparativoHtml += '<p class="txt">Ainda sem treino anterior pra comparar, esse será o primeiro.</p>';
  } else {
    categorias.forEach(function(cat){
      const antes = snapshotAntes.totais[cat] || 0;
      const depois = snapshotDepois.totais[cat] || 0;
      const seta = depois > antes ? '↑' : (depois < antes ? '↓' : '=');
      comparativoHtml += '<p class="txt">' + cat + ': ' + antes + ' → ' + depois + ' séries ' + seta + '</p>';
    });
  }
  comparativoHtml += '<p class="txt" style="color:var(--gold-soft);margin-top:6px;">Duração total da semana: ' + snapshotAntes.duracaoTotal + 'min → ' + snapshotDepois.duracaoTotal + 'min</p></div>';

  area.innerHTML = '<p class="section-label" style="margin-top:14px;">O que vai mudar com esse ajuste</p>' +
    comparativoHtml +
    renderChecklistPrescricao(resultadoValidacao) +
    '<button class="btn-gold" onclick="confirmarGerarTreinoPreview()">Confirmar e gerar esse novo treino</button>';
}

function mostrarAvisoSessaoInvalida(){
  if(document.getElementById('overlay-sessao-invalida')) return; // já está mostrando, não duplica
  const overlay = document.createElement('div');
  overlay.id = 'overlay-sessao-invalida';
  overlay.style.cssText = 'position:fixed;inset:0;background:rgba(0,0,0,0.92);z-index:9999999;display:flex;flex-direction:column;align-items:center;justify-content:center;padding:24px;text-align:center;';
  overlay.innerHTML =
    '<p style="font-size:17px;font-weight:700;color:#E2A33D;margin-bottom:10px;">Sua sessão não está mais válida</p>' +
    '<p style="font-size:13px;color:#ccc;max-width:320px;margin-bottom:20px;">Por isso o último ajuste não foi salvo de verdade no banco. Precisa entrar de novo pra continuar editando com segurança.</p>' +
    '<button class="btn-gold" style="width:auto;padding:12px 28px;" onclick="document.getElementById(\'overlay-sessao-invalida\').remove(); reautenticarComoPersonal();">Entrar de novo</button>';
  document.body.appendChild(overlay);
}

async function reautenticarComoPersonal(){
  document.getElementById('login-personal-email').value = EMAIL_PERSONAL;
  document.getElementById('login-personal-senha').value = SENHA_PERSONAL;
  await loginPersonal();
}

async function garantirSessaoValida(){
  if(!supabaseClient) return false;
  try {
    const { data: sessaoData } = await supabaseClient.auth.getSession();
    const sessao = sessaoData ? sessaoData.session : null;
    if(!sessao || !sessao.user){ mostrarAvisoSessaoInvalida(); return false; }

    // Não basta ter sessão — precisa bater como "personal" de verdade na tabela perfis, senão o RLS bloqueia mesmo com sessão válida
    const { data: perfilRow, error: erroPerfil } = await supabaseClient.from('perfis').select('tipo').eq('id', sessao.user.id).maybeSingle();
    if(erroPerfil || !perfilRow || perfilRow.tipo !== 'personal'){
      mostrarAvisoSessaoInvalida();
      return false;
    }
    return true;
  } catch(erroDeRede){
    mostrarAvisoSessaoInvalida();
    return false;
  }
}

async function sincronizarTreinoComSupabase(a){
  if(!supabaseClient){ mostrarConfirmacaoSalvamento(false, 'Sem conexão com o Supabase agora.'); return { ok: false, motivo: 'sem conexão' }; }
  if(!(await garantirSessaoValida())){ mostrarConfirmacaoSalvamento(false, 'Sessão inválida — o ajuste não foi salvo. Entra de novo.'); return { ok: false, motivo: 'sessão inválida' }; }
  if(!a.email){ mostrarConfirmacaoSalvamento(false, 'Essa aluna não tem e-mail cadastrado, não dá pra salvar no banco.'); return { ok: false, motivo: 'sem e-mail' }; }
  try {
    let { data: alunaRow, error: erroBusca } = await supabaseClient.from('alunas').select('auth_id').eq('email', a.email).maybeSingle();
    if(erroBusca){ mostrarConfirmacaoSalvamento(false, 'Erro ao buscar aluna no banco: ' + erroBusca.message); return { ok: false, motivo: erroBusca.message }; }

    if(!alunaRow){
      // Nunca desiste por falta de cadastro — cria na hora, com os dados que já temos localmente
      const { error: erroCriar } = await supabaseClient.from('alunas').upsert({
        email: a.email, nome: a.nome, telefone: a.telefone || '',
        nivel: a.nivel || 'Iniciante', freq: a.freq || '3x por semana', piramide: a.piramide || '',
        objetivo: a.objetivo || '', restricoes: a.restricoes || '', academia: a.academia || '',
        data_anamnese: a.dataAnamnese || null
      }, { onConflict: 'email' });
      if(erroCriar){ mostrarConfirmacaoSalvamento(false, 'Não consegui criar o cadastro dela: ' + erroCriar.message); return { ok: false, motivo: erroCriar.message }; }
      alunaRow = { auth_id: null };
    }

    if(alunaRow.auth_id){
      // Ela já tem login: salva no lugar "oficial" (tabela treinos), que é o que o app dela lê ao entrar
      const saidaTreino = await salvarTreinoNoSupabase(alunaRow.auth_id, a.treinoAtual);
      return saidaTreino && saidaTreino.sucesso ? { ok: true, verificado: true } : { ok: false, motivo: saidaTreino ? saidaTreino.erro : 'falha ao salvar' };
    } else {
      // Sempre salva também aqui, pelo e-mail, com ou sem login — assim o treino nunca se perde,
      // mesmo que você feche o app antes de gerar o acesso dela
      const { error: erroBackup } = await supabaseClient.from('alunas').update({ treino_atual_backup: a.treinoAtual }).eq('email', a.email);
      if(erroBackup){ mostrarConfirmacaoSalvamento(false, 'Erro ao salvar backup: ' + erroBackup.message); return { ok: false, motivo: erroBackup.message }; }
      mostrarConfirmacaoSalvamento(true, 'Salvo no backup (login ainda não está vinculado a essa aluna na tabela).');
      return { ok: true, verificado: false, motivo: 'salvo só no backup, sem login ainda' };
    }
  } catch(erroDeRede){
    mostrarConfirmacaoSalvamento(false, 'Erro de conexão: ' + erroDeRede.message);
    return { ok: false, motivo: erroDeRede.message };
  }
}

function confirmarGerarTreinoPreview(){
  if(!treinoPreviewPendente) return;
  const a = alunasPersonal.find(function(x){ return x.nome === treinoPreviewPendente.nomeAluna; });
  if(!a) return;
  const nomesDias = ['Segunda','Terça','Quarta','Quinta','Sexta','Sábado','Domingo'];
  const semanaComNome = nomesDias.map(function(nome, i){
    if(i < treinoPreviewPendente.semana.length) return Object.assign({ n: nome }, treinoPreviewPendente.semana[i]);
    return { n: nome, foco: 'Descanso', descanso: true, ex: [] };
  });
  a.treinoAtual = { fase: (a.treinoAtual && a.treinoAtual.fase) || 'A definir', volume: 'Gerado automaticamente, revise antes de confirmar', dias: semanaComNome };
  sincronizarTreinoComSupabase(a);
  treinoPreviewPendente = null;
  const i = alunasPersonal.indexOf(a);
  openAlunaDetail(i);
}

const timersSalvamentoPerfil = {};

function salvarPerfilAlunaNoSupabase(nomeAluna){
  // Espera inteligente: se chamar de novo antes de 500ms, cancela o anterior e reinicia a espera.
  // Isso garante que, mesmo mexendo em várias coisas rápido, só sai UM salvamento por vez, sempre com o estado mais completo e atual.
  // É independente por aluna (cada uma tem seu próprio temporizador), então mexer em duas alunas ao mesmo tempo não interfere uma na outra.
  if(timersSalvamentoPerfil[nomeAluna]) clearTimeout(timersSalvamentoPerfil[nomeAluna]);
  timersSalvamentoPerfil[nomeAluna] = setTimeout(function(){
    delete timersSalvamentoPerfil[nomeAluna];
    executarSalvamentoPerfilAluna(nomeAluna);
  }, 500);
}

async function executarSalvamentoPerfilAluna(nomeAluna){
  if(!supabaseClient) return;
  try {
    const a = alunasPersonal.find(function(x){ return x.nome === nomeAluna; });
    if(!a || !a.email) return;
    await supabaseClient.from('alunas').upsert({
      email: a.email,
      nome: a.nome,
      telefone: a.telefone || '',
      nivel: a.nivel || 'Iniciante',
      freq: a.freq || '3x por semana',
      piramide: a.piramide || '',
      objetivo: a.objetivo || '',
      restricoes: a.restricoes || '',
      academia: a.academia || '',
      idade: a.idade || null,
      dados_extras: {
        estagioFunilManual: a.estagioFunilManual || null,
        statusPlanoManual: a.statusPlanoManual || null,
        patologiaConfirmada: a.patologiaConfirmada || null,
        duvidasSinalizadas: a.duvidasSinalizadas || [],
        diasNoProgramaFunil: a.diasNoProgramaFunil || null,
        tecnicaAprovada: a.tecnicaAprovada || {},
        cicloPerguntado: a.cicloPerguntado || false,
        cicloInfo: a.cicloInfo || null,
        direcionamentoQuadriceps: a.direcionamentoQuadriceps || null,
        direcionamentoGluteo: a.direcionamentoGluteo || null,
        desviosPosturaisConfirmados: a.desviosPosturaisConfirmados || [],
        dataFechouPlano: a.dataFechouPlano || null,
        duracaoPlanoDias: a.duracaoPlanoDias || null,
        valorPlano: a.valorPlano != null ? a.valorPlano : null,
        statusPagamento: a.statusPagamento || null,
        tabataSugestaoRespondida: a.tabataSugestaoRespondida || false,
        composicaoAtual: a.composicaoAtual || null,
        queixaDor: a.queixaDor || false,
        regiaoQueixa: a.regiaoQueixa || null,
        ambienteTreino: a.ambienteTreino || 'Academia',
        dataNascimento: a.dataNascimento || null,
        aceitaAvisos: a.aceitaAvisos != null ? a.aceitaAvisos : null,
        contadorAberturas: a.contadorAberturas || 0,
        sinalRiscoEmocional: a.sinalRiscoEmocional || null,
        progressaoManualForcada: a.progressaoManualForcada || 0,
        composicaoHistorico: a.composicaoHistorico || [],
        pesoHistorico: a.pesoHistorico || []
      }
    }, { onConflict: 'email' });
  } catch(erroDeRede){
    console.warn('Sem conexão agora, edição ficou salva só localmente por enquanto:', erroDeRede);
  }
}

function editarNivelAluna(nomeAluna, novoNivel){
  const a = alunasPersonal.find(function(x){ return x.nome === nomeAluna; });
  if(!a) return;
  a.nivel = novoNivel;
  const i = alunasPersonal.indexOf(a);
  openAlunaDetail(i);
  mostrarPreviewMudancaTreino(nomeAluna);
  salvarPerfilAlunaNoSupabase(nomeAluna);
}

function editarAmbienteTreinoAluna(nomeAluna, novoAmbiente){
  const a = alunasPersonal.find(function(x){ return x.nome === nomeAluna; });
  if(!a) return;
  a.ambienteTreino = novoAmbiente;
  const i = alunasPersonal.indexOf(a);
  openAlunaDetail(i);
  mostrarPreviewMudancaTreino(nomeAluna);
  salvarPerfilAlunaNoSupabase(nomeAluna);
}

function editarFrequenciaAluna(nomeAluna, novaFreq){
  const a = alunasPersonal.find(function(x){ return x.nome === nomeAluna; });
  if(!a) return;
  a.freq = novaFreq;
  const i = alunasPersonal.indexOf(a);
  openAlunaDetail(i);
  mostrarPreviewMudancaTreino(nomeAluna);
  salvarPerfilAlunaNoSupabase(nomeAluna);
}

function editarDataNascimentoAluna(nomeAluna, novaData){
  const a = alunasPersonal.find(function(x){ return x.nome === nomeAluna; });
  if(!a) return;
  a.dataNascimento = novaData;
  salvarPerfilAlunaNoSupabase(nomeAluna);
  renderCentralDeAvisos();
}

const secoesColapsaveisAbertas = {}; // guarda estado (aberta/fechada) de cada seção, sobrevive a recarregar a ficha

function renderSecaoColapsavel(titulo, conteudoHtml, idUnico){
  const estaAberta = !!secoesColapsaveisAbertas[idUnico];
  return '<div class="section-colapsavel" style="margin-top:22px;">' +
    '<div style="display:flex;justify-content:space-between;align-items:center;cursor:pointer;" onclick="alternarSecaoColapsavel(\'' + idUnico + '\')">' +
      '<p class="section-label" style="margin:0;">' + titulo + '</p>' +
      '<i class="ti ti-chevron-down" id="chevron-' + idUnico + '" style="color:var(--gold-soft);font-size:16px;transition:transform .2s;' + (estaAberta ? 'transform:rotate(180deg);' : '') + '"></i>' +
    '</div>' +
    '<div id="conteudo-' + idUnico + '" style="display:' + (estaAberta ? 'block' : 'none') + ';margin-top:8px;">' + conteudoHtml + '</div>' +
  '</div>';
}

function alternarSecaoColapsavel(idUnico){
  const conteudo = document.getElementById('conteudo-' + idUnico);
  const chevron = document.getElementById('chevron-' + idUnico);
  if(!conteudo) return;
  const abrindo = conteudo.style.display === 'none';
  conteudo.style.display = abrindo ? 'block' : 'none';
  secoesColapsaveisAbertas[idUnico] = abrindo;
  if(chevron) chevron.style.transform = abrindo ? 'rotate(180deg)' : 'rotate(0deg)';
}

function renderResumoMetodologiaAutomatica(a){
  const piramideInfo = extrairEnfaseSecundaria(a.piramide);
  const enfaseDetectada = piramideInfo.enfase;
  const secundarioDetectado = piramideInfo.secundario;
  const dQuad = a.direcionamentoQuadriceps || 'nenhum';
  const dGluteo = a.direcionamentoGluteo || 'nenhum';
  const desvios = (a.desviosPosturaisConfirmados || []).length;
  const blocoInfo = calcularBlocoAtual(a);

  return renderSecaoColapsavel('Pirâmide de prioridade', '<div class="info-box"><p class="txt" style="font-weight:600;">' + (a.piramide || 'Não respondida') + '</p></div>', 'piramide-' + a.nome.replace(/[^a-zA-Z0-9]/g,'')) +
    renderSecaoColapsavel('Nossa metodologia, agindo automaticamente',
      '<div class="info-box">' +
        '<p class="txt">✓ Ênfase interpretada da pirâmide: <b>' + enfaseDetectada + '</b> · Secundário: <b>' + secundarioDetectado + '</b></p>' +
        '<p class="txt">✓ Nível: <b>' + (a.nivel || 'A definir') + '</b> · Frequência: <b>' + (a.freq || 'A definir') + '</b></p>' +
        '<p class="txt">✓ Bloco atual: <b>' + blocoInfo.bloco + '</b></p>' +
        (blocoInfo.blocoTecnico === 'deload' || blocoInfo.volumeReduzido ? '<p class="txt" style="color:#E2A33D;">⚠️ ' + blocoInfo.descricao + '</p>' : '') +
        '<p class="txt">' + (dQuad !== 'nenhum' ? '✓ Direcionamento de quadríceps ativo: <b>' + dQuad + '</b>' : 'Sem direcionamento específico de quadríceps') + '</p>' +
        '<p class="txt">' + (dGluteo !== 'nenhum' ? '✓ Direcionamento de glúteo ativo: <b>' + dGluteo + '</b>' : 'Sem direcionamento específico de glúteo') + '</p>' +
        '<p class="txt" style="margin-bottom:0;">' + (desvios > 0 ? '✓ ' + desvios + ' desvio(s) postural(is) confirmado(s), bloco de correção será incluído' : 'Nenhum desvio postural confirmado ainda') + '</p>' +
      '</div>', 'metodologia-' + a.nome.replace(/[^a-zA-Z0-9]/g,''));
}

function confirmarProgressaoManual(nomeAluna){
  const a = alunasPersonal.find(function(x){ return x.nome === nomeAluna; });
  if(!a) return;
  // Progressão manual: você (Personal) decide que está pronta pra evoluir agora, mesmo sem
  // dado suficiente registrado ainda. Cada clique aqui força uma variação real de exercício e
  // um degrau a mais de volume, independente da semana/histórico dela.
  a.progressaoManualForcada = (a.progressaoManualForcada || 0) + 1;
  salvarPerfilAlunaNoSupabase(nomeAluna);
  gerarTreinoAutomaticoParaAluna(nomeAluna);
  if(a.telefone) enviarWhatsApp(a.telefone, templatesAvisos.treinoProgredido(a));
  enviarAvisoInApp(nomeAluna, 'Seu treino acabou de ser atualizado, com evolução de carga/variedade. Já pode conferir! 💪', 'ajuste_treino');
}

function calcularVolumeTotalDoTreino(diasTreino){
  if(!diasTreino) return { series: 0, exercicios: 0 };
  let series = 0, exercicios = 0;
  diasTreino.forEach(function(d){
    (d.ex || []).forEach(function(linha){
      exercicios++;
      const match = linha.match(/(\d+)\s*x\s*\d+/i);
      series += match ? parseInt(match[1], 10) : 1;
    });
  });
  return { series: series, exercicios: exercicios };
}

function mostrarDiagnosticoParaProgredir(nomeAluna){
  const a = alunasPersonal.find(function(x){ return x.nome === nomeAluna; });
  if(!a) return;
  const container = document.getElementById('diagnostico-progressao-area');
  if(!container) return;
  if(container.innerHTML){ container.innerHTML = ''; return; } // clique de novo fecha

  const diagnostico = gerarDiagnosticoInterno(a);
  const prog = getProgressoAluna(a.nome);
  const stats = calcularEstatisticasAluna(a.nome);

  const volumeAtual = calcularVolumeTotalDoTreino(a.treinoAtual ? a.treinoAtual.dias : null);

  // Simula o treino novo (sem salvar/aplicar ainda) só pra comparar volume, exatamente como ficaria progredindo
  const progressaoSimulada = (a.progressaoManualForcada || 0) + 1;
  const aluna_simulada = Object.assign({}, a, { progressaoManualForcada: progressaoSimulada });
  const perfilNovo = construirPerfilAluna(aluna_simulada);
  const treinoNovoSimulado = gerarTreinoSemanal(perfilNovo);
  const volumeNovo = calcularVolumeTotalDoTreino(treinoNovoSimulado);

  const diffSeries = volumeNovo.series - volumeAtual.series;
  const progressoesRegistradas = stats.temDados ? stats.progressoes : 0;
  const totalExerciciosComHistorico = Object.keys(prog.historico || {}).length;

  container.innerHTML = '<div class="info-box" style="margin-top:10px;">' +
    '<p class="lbl">Comparativo — treino atual vs. treino progredido</p>' +
    '<div style="display:flex;gap:10px;margin:8px 0;">' +
      '<div style="flex:1;background:var(--card-2);border-radius:10px;padding:10px;text-align:center;">' +
        '<p style="font-size:10px;color:var(--text-faint);margin:0 0 4px;">ATUAL</p>' +
        '<p style="font-size:20px;font-weight:700;margin:0;">' + volumeAtual.series + '</p>' +
        '<p style="font-size:10px;color:var(--text-faint);margin:2px 0 0;">séries · ' + volumeAtual.exercicios + ' exercícios</p>' +
      '</div>' +
      '<div style="flex:1;background:var(--card-2);border-radius:10px;padding:10px;text-align:center;border:1px solid var(--border-strong);">' +
        '<p style="font-size:10px;color:var(--gold-soft);margin:0 0 4px;">PROGREDIDO</p>' +
        '<p style="font-size:20px;font-weight:700;color:var(--gold-soft);margin:0;">' + volumeNovo.series + '</p>' +
        '<p style="font-size:10px;color:var(--text-faint);margin:2px 0 0;">séries · ' + volumeNovo.exercicios + ' exercícios</p>' +
      '</div>' +
    '</div>' +
    '<p class="txt" style="text-align:center;font-size:11px;color:' + (diffSeries > 0 ? 'var(--success)' : 'var(--text-faint)') + ';">' + (diffSeries > 0 ? '▲ +' + diffSeries + ' séries' : (diffSeries < 0 ? diffSeries + ' séries' : 'mesmo volume, foco em variedade')) + ' em relação ao atual</p>' +
    '<p class="section-label" style="margin-top:12px;font-size:11px;">Scout dela até agora</p>' +
    '<p class="txt"><b>Semana atual (DNA):</b> ' + prog.semana + ' · <b>Frequência real:</b> ' + (stats.temDados ? Math.round((stats.totalConcluidos/stats.totalPlanejado)*100) + '% de constância' : 'ainda sem dados suficientes') + '</p>' +
    '<p class="txt"><b>Progressões de carga registradas:</b> ' + progressoesRegistradas + ' · <b>Exercícios com histórico:</b> ' + totalExerciciosComHistorico + '</p>' +
    '<p class="txt" style="margin-top:4px;"><b>Estratégia agora:</b> ' + diagnostico.estrategiaAtual + '</p>' +
    '<p class="txt" style="font-size:11px;color:var(--text-faint);">' + diagnostico.justificativaEstrategia + '</p>' +
    '<p class="txt" style="margin-top:4px;"><b>Fase:</b> ' + diagnostico.faseAtual + '</p>' +
    (diagnostico.pontosFortes.length ? '<p class="txt" style="font-size:11px;color:var(--success);margin-top:4px;">✓ ' + diagnostico.pontosFortes.join(' · ') + '</p>' : '') +
    (diagnostico.gargalos.length ? '<p class="txt" style="font-size:11px;color:var(--gold-soft);margin-top:4px;">⚠ ' + diagnostico.gargalos.join(' · ') + '</p>' : '') +
    '</div>' +
    '<button class="btn-gold" style="margin-top:8px;" onclick="confirmarProgressaoManual(\'' + a.nome.replace(/'/g,"\\'") + '\')">Confirmar e progredir o treino</button>';
}

// Núcleo reaproveitável: calcula e atribui o treino, sem nenhum efeito de navegação/tela —
// usado tanto pelo botão individual quanto pela geração em massa.
async function construirEAtribuirTreino(nomeAluna){
  const a = alunasPersonal.find(function(x){ return x.nome === nomeAluna; });
  if(!a) return { ok: false, motivo: 'aluna não encontrada' };

  const perfil = construirPerfilAluna(a);
  const semana = gerarTreinoSemanal(perfil);
  const nomesDias = ['Segunda','Terça','Quarta','Quinta','Sexta','Sábado','Domingo'];
  const semanaComNome = nomesDias.map(function(nome, i){
    if(i < semana.length) return Object.assign({ n: nome }, semana[i]);
    return { n: nome, foco: 'Descanso', descanso: true, ex: [] };
  });

  a.treinoAtual = { fase: (a.treinoAtual && a.treinoAtual.fase) || 'A definir', volume: 'Gerado automaticamente, revise antes de confirmar', dias: semanaComNome };
  const saidaSalvamento = await sincronizarTreinoComSupabase(a);

  const resultado = validarPrescricao(perfil, semana);
  return { ok: true, resultado: resultado, salvamento: saidaSalvamento };
}

function gerarTreinoAutomaticoParaAluna(nomeAluna){
  construirEAtribuirTreino(nomeAluna).then(function(saida){
    if(!saida.ok) return;
    const a = alunasPersonal.find(function(x){ return x.nome === nomeAluna; });
    const i = alunasPersonal.indexOf(a);
    openAlunaDetail(i);
    const areaValidacao = document.getElementById('validacao-treino-area');
    if(areaValidacao) areaValidacao.innerHTML = renderChecklistPrescricao(saida.resultado);
  });
}

// ===== GERAÇÃO EM MASSA =====
function mostrarConfigRanking(){
  const area = document.getElementById('config-ranking-area');
  if(area.innerHTML){ area.innerHTML = ''; return; }
  area.innerHTML = '<div class="info-box">' +
    '<div class="form-group"><label class="form-label">Meta de pontos da comunidade</label><input class="form-input" id="input-meta-pontos" type="number" value="' + metaComunidadePontos + '"></div>' +
    '<div class="form-group"><label class="form-label">O que desbloqueia ao bater a meta?</label><input class="form-input" id="input-meta-recompensa" value="' + metaComunidadeRecompensa.replace(/"/g,'&quot;') + '"></div>' +
    '<button class="btn-gold" onclick="salvarConfigRanking()">Salvar</button>' +
  '</div>';
}

async function salvarConfigRanking(){
  metaComunidadePontos = parseInt(document.getElementById('input-meta-pontos').value, 10) || metaComunidadePontos;
  metaComunidadeRecompensa = document.getElementById('input-meta-recompensa').value.trim() || metaComunidadeRecompensa;
  await salvarCatalogoPersonal('config_ranking', 'meta', { meta: metaComunidadePontos, recompensa: metaComunidadeRecompensa });
  document.getElementById('config-ranking-area').innerHTML = '<p class="txt" style="color:var(--success);">✓ Meta atualizada.</p>';
}

// Auditoria de volume mínimo de posteriores: revisa treinos já existentes (gerados antes da regra
// entrar em vigor) e aponta quais alunas ainda não têm nenhum exercício de posterior no treino atual.
async function auditarVolumePosteriores(){
  const area = document.getElementById('auditoria-posteriores-area');
  if(!area) return;
  area.innerHTML = '<p class="txt" style="color:var(--text-faint);">Revisando treinos de todas as alunas...</p>';

  const comTreino = alunasPersonal.filter(function(a){ return a.treinoAtual && a.treinoAtual.dias; });
  const semPosterior = [];

  comTreino.forEach(function(a){
    const temPosterior = a.treinoAtual.dias.some(function(d){
      return (d.ex || []).some(function(linhaOriginal){
        const linhas = linhaOriginal.indexOf('|||') !== -1 ? linhaOriginal.split('|||').slice(1).map(function(l){ return l.trim(); }) : [linhaOriginal];
        return linhas.some(function(linha){
          const nomeEx = linha.split(' · ')[0];
          const exBanco = buscarExercicioNoBanco(nomeEx);
          return exBanco && exBanco.grupo === 'Isquiotibiais';
        });
      });
    });
    if(!temPosterior) semPosterior.push(a.nome);
  });

  if(semPosterior.length === 0){
    area.innerHTML = '<div class="info-box" style="border-color:var(--success);"><p class="lbl" style="color:var(--success);">✓ Nenhum problema encontrado</p><p class="txt">Revisei ' + comTreino.length + ' treino(s) — todas têm pelo menos 1 exercício de posterior.</p></div>';
    return;
  }

  area.innerHTML = '<div class="info-box" style="border-color:#E2A33D;"><p class="lbl" style="color:#E2A33D;">⚠ ' + semPosterior.length + ' aluna(s) sem posterior no treino atual</p>' +
    '<p class="txt" style="font-size:11px;color:var(--text-faint);">' + semPosterior.join(', ') + '</p>' +
    '<p class="txt" style="font-size:11px;margin-top:8px;">Isso são treinos gerados antes da regra de volume mínimo entrar em vigor. Use "Gerar/progredir treino de todas" no Dashboard pra corrigir todas de uma vez, já com a regra nova aplicada.</p>' +
  '</div>';
}

async function verificarReplicasDeTreino(){
  const area = document.getElementById('replicas-treino-area');
  area.innerHTML = '<p class="txt" style="color:var(--text-faint);">Cruzando os treinos de todas as alunas...</p>';

  const comTreino = alunasPersonal.filter(function(a){ return a.treinoAtual && a.treinoAtual.dias; });
  const fingerprints = {};

  comTreino.forEach(function(a){
    const assinatura = a.treinoAtual.dias.map(function(d){
      return (d.ex || []).map(function(linha){ return linha.split(' · ')[0].trim().toUpperCase(); }).join('|');
    }).join('||');
    if(!fingerprints[assinatura]) fingerprints[assinatura] = [];
    fingerprints[assinatura].push(a.nome);
  });

  const replicasEncontradas = Object.values(fingerprints).filter(function(grupo){ return grupo.length > 1; });

  if(replicasEncontradas.length === 0){
    area.innerHTML = '<div class="info-box" style="border-color:var(--success);"><p class="lbl" style="color:var(--success);">✓ Nenhuma réplica encontrada</p><p class="txt">Cruzei ' + comTreino.length + ' treinos — todos são únicos, exercício por exercício, dia por dia.</p></div>';
    return;
  }

  let html = '<div class="info-box" style="border-color:#E2A33D;"><p class="lbl" style="color:#E2A33D;">⚠ ' + replicasEncontradas.length + ' grupo(s) de réplica encontrado(s)</p>' +
    '<p class="txt" style="font-size:11px;color:var(--text-faint);">Essas alunas têm o treino idêntico, exercício por exercício, em todos os dias — vale progredir uma delas manualmente pra diferenciar.</p></div>';
  replicasEncontradas.forEach(function(grupo){
    html += '<div class="list-item" style="flex-direction:column;align-items:stretch;margin-top:8px;">' +
      '<p style="font-size:12px;font-weight:600;margin:0 0 4px;">' + grupo.length + ' alunas com o mesmo treino:</p>' +
      '<p style="font-size:12px;color:var(--text-faint);margin:0;">' + grupo.join(', ') + '</p>' +
    '</div>';
  });
  area.innerHTML = html;
}

async function iniciarGeracaoEmMassa(){
  const elegiveis = alunasPersonal.filter(function(a){ return a.status !== 'lead' && a.email; });
  if(!confirm('Isso vai gerar ou progredir o treino de ' + elegiveis.length + ' aluna(s) de uma vez, seguindo a metodologia individual de cada uma. Pode levar alguns minutos. Continuar?')) return;

  const area = document.getElementById('geracao-massa-area');
  const resultados = { geradas: 0, progredidas: 0, verificadas: 0, apenasBackup: 0, erros: [] };

  for(let idx = 0; idx < elegiveis.length; idx++){
    const a = elegiveis[idx];
    area.innerHTML = '<div class="info-box"><p class="lbl">Processando ' + (idx+1) + ' de ' + elegiveis.length + '...</p><p class="txt">' + a.nome + '</p></div>';
    try {
      const jaTinhaTreino = !!a.treinoAtual;
      if(jaTinhaTreino) a.progressaoManualForcada = (a.progressaoManualForcada || 0) + 1;
      const saida = await construirEAtribuirTreino(a.nome);
      if(!saida.ok){ resultados.erros.push({ nome: a.nome, motivo: saida.motivo }); continue; }
      if(!saida.salvamento || !saida.salvamento.ok){
        resultados.erros.push({ nome: a.nome, motivo: 'gerou mas NÃO salvou: ' + (saida.salvamento ? saida.salvamento.motivo : 'desconhecido') });
        continue;
      }
      if(jaTinhaTreino) resultados.progredidas++; else resultados.geradas++;
      if(saida.salvamento.verificado) resultados.verificadas++; else resultados.apenasBackup++;
    } catch(erro){
      resultados.erros.push({ nome: a.nome, motivo: erro.message });
    }
  }

  area.innerHTML = '<div class="info-box" style="border-color:var(--success);">' +
    '<p class="lbl" style="color:var(--success);">✓ Concluído</p>' +
    '<p class="txt">' + resultados.geradas + ' geradas pela primeira vez · ' + resultados.progredidas + ' progredidas</p>' +
    '<p class="txt" style="font-size:11px;color:var(--success);margin-top:4px;">✓ ' + resultados.verificadas + ' confirmadas de verdade (lidas de volta do banco)</p>' +
    (resultados.apenasBackup > 0 ? '<p class="txt" style="font-size:11px;color:var(--gold-soft);margin-top:2px;">⚠ ' + resultados.apenasBackup + ' salvas só no backup (login ainda não vinculado a essas)</p>' : '') +
    '<p class="txt" style="font-size:11px;color:' + (resultados.erros.length > 0 ? '#E2A33D' : 'var(--text-faint)') + ';margin-top:4px;">' + resultados.erros.length + ' com erro real</p>' +
    (resultados.erros.length > 0 ? '<p class="txt" style="font-size:11px;color:#E2A33D;margin-top:6px;">' + resultados.erros.map(function(e){ return e.nome + ' (' + e.motivo + ')'; }).join('<br>') + '</p>' : '') +
  '</div>';
}

function editarSeriesReps(diaIndex, exIndex, novoValor){
  const a = alunaAberta;
  const nomeAtual = a.treinoAtual.dias[diaIndex].ex[exIndex].split(' · ')[0];
  a.treinoAtual.dias[diaIndex].ex[exIndex] = nomeAtual + ' · ' + novoValor.trim();
  if(typeof dias !== 'undefined' && dias[diaIndex] && dias[diaIndex].ex[exIndex]){
    dias[diaIndex].ex[exIndex] = nomeAtual + ' · ' + novoValor.trim();
  }
  sincronizarTreinoComSupabase(a);
}

// ===== ARRASTAR PRA REORDENAR (funciona com toque e mouse, sem depender de "drag and drop" nativo) =====
let arrastandoExercicio = null;

function iniciarArrastarExercicio(ev){
  const alca = ev.currentTarget;
  const diaIndex = parseInt(alca.getAttribute('data-dia'), 10);
  const exIndexInicial = parseInt(alca.getAttribute('data-ex'), 10);
  const linha = alca.closest('.exercicio-linha');
  if(!linha) return;
  ev.preventDefault();

  arrastandoExercicio = {
    diaIndex: diaIndex,
    exAtual: exIndexInicial,
    linha: linha,
    yInicial: ev.clientY,
    alturaLinha: linha.offsetHeight
  };
  linha.style.position = 'relative';
  linha.style.zIndex = '10';
  linha.style.boxShadow = '0 6px 16px rgba(0,0,0,0.4)';
  linha.style.background = 'var(--card-2)';
  alca.style.cursor = 'grabbing';

  document.addEventListener('pointermove', moverDuranteArraste);
  document.addEventListener('pointerup', finalizarArraste);
}

function moverDuranteArraste(ev){
  if(!arrastandoExercicio) return;
  const deltaY = ev.clientY - arrastandoExercicio.yInicial;
  arrastandoExercicio.linha.style.transform = 'translateY(' + deltaY + 'px)';

  // Cruzou mais da metade de uma linha vizinha? Troca de posição na hora, só localmente (sem redesenhar a tela ainda)
  const limiar = arrastandoExercicio.alturaLinha / 2;
  if(deltaY > limiar){
    trocarPosicaoLocalSemRedesenhar(arrastandoExercicio.diaIndex, arrastandoExercicio.exAtual, 1);
    arrastandoExercicio.exAtual += 1;
    arrastandoExercicio.yInicial += arrastandoExercicio.alturaLinha;
    arrastandoExercicio.linha.style.transform = 'translateY(0px)';
  } else if(deltaY < -limiar){
    trocarPosicaoLocalSemRedesenhar(arrastandoExercicio.diaIndex, arrastandoExercicio.exAtual, -1);
    arrastandoExercicio.exAtual -= 1;
    arrastandoExercicio.yInicial -= arrastandoExercicio.alturaLinha;
    arrastandoExercicio.linha.style.transform = 'translateY(0px)';
  }
}

function trocarPosicaoLocalSemRedesenhar(diaIndex, exIndex, direcao){
  const a = alunaAberta;
  const diaAtual = a.treinoAtual.dias[diaIndex];
  const novoIndex = exIndex + direcao;
  if(novoIndex < 0 || novoIndex >= diaAtual.ex.length) return;
  function trocarPosicao(arr, i, j){ if(!arr) return; const tmp = arr[i]; arr[i] = arr[j]; arr[j] = tmp; }
  trocarPosicao(diaAtual.ex, exIndex, novoIndex);
  trocarPosicao(diaAtual.metodos, exIndex, novoIndex);
  if(typeof dias !== 'undefined' && dias[diaIndex]){
    trocarPosicao(dias[diaIndex].ex, exIndex, novoIndex);
    trocarPosicao(dias[diaIndex].metodos, exIndex, novoIndex);
  }
}

function finalizarArraste(){
  if(!arrastandoExercicio) return;
  const diaIndex = arrastandoExercicio.diaIndex;
  document.removeEventListener('pointermove', moverDuranteArraste);
  document.removeEventListener('pointerup', finalizarArraste);
  arrastandoExercicio = null;
  // Só agora, com o gesto todo terminado, salva de verdade e redesenha a tela uma vez só
  sincronizarTreinoComSupabase(alunaAberta);
  atualizarDiaPersonalNaTela(diaIndex);
}

function moverExercicioTreino(diaIndex, exIndex, direcao){
  const a = alunaAberta;
  const diaAtual = a.treinoAtual.dias[diaIndex];
  const novoIndex = exIndex + direcao;
  if(novoIndex < 0 || novoIndex >= diaAtual.ex.length) return; // já está na ponta, não faz nada

  function trocarPosicao(arr, i, j){
    if(!arr) return;
    const tmp = arr[i]; arr[i] = arr[j]; arr[j] = tmp;
  }

  trocarPosicao(diaAtual.ex, exIndex, novoIndex);
  trocarPosicao(diaAtual.metodos, exIndex, novoIndex); // o método aplicado acompanha o exercício, não fica preso na posição

  if(typeof dias !== 'undefined' && dias[diaIndex]){
    trocarPosicao(dias[diaIndex].ex, exIndex, novoIndex);
    trocarPosicao(dias[diaIndex].metodos, exIndex, novoIndex);
  }

  sincronizarTreinoComSupabase(a);
  atualizarDiaPersonalNaTela(diaIndex);
}

function removerExercicioTreino(diaIndex, exIndex){
  const a = alunaAberta;
  a.treinoAtual.dias[diaIndex].ex.splice(exIndex, 1);
  if(typeof dias !== 'undefined' && dias[diaIndex]) dias[diaIndex].ex.splice(exIndex, 1);
  sincronizarTreinoComSupabase(a);
  atualizarDiaPersonalNaTela(diaIndex);
}

function abrirSelecaoGrupoParaAdicionar(diaIndex){
  const grupos = ['Costas','Peito','Ombros','Bíceps','Tríceps','Quadríceps','Glúteos','Isquiotibiais','Panturrilha','Abdômen'];
  const form = document.getElementById('add-exercicio-form-' + diaIndex);
  if(!form) return;
  form.innerHTML = '<p class="lbl" style="margin-top:8px;">De qual grupo muscular?</p>' +
    '<div class="chip-list" style="margin:6px 0;">' +
    grupos.map(function(g){ return '<span class="chip" style="cursor:pointer;" onclick="abrirSelecaoExercicioParaAdicionar(' + diaIndex + ',\'' + g + '\')">' + g + '</span>'; }).join('') +
    '</div>';
}

function abrirSelecaoExercicioParaAdicionar(diaIndex, grupo){
  const opcoes = exerciciosBanco.filter(function(e){ return e.grupo === grupo || e.categoria === grupo; });
  const form = document.getElementById('add-exercicio-form-' + diaIndex);
  if(!form) return;
  form.innerHTML = '<p class="lbl" style="margin-top:8px;">Exercício de ' + grupo + '</p>' +
    '<div class="form-group"><select class="form-select" id="add-exercicio-select-' + diaIndex + '">' +
      opcoes.map(function(e){ return '<option value="' + e.nome.replace(/"/g,'') + '">' + e.nome + '</option>'; }).join('') +
    '</select></div>' +
    '<button class="btn-gold" style="width:auto;padding:8px 14px;margin:0;font-size:12px;" onclick="confirmarAdicionarExercicio(' + diaIndex + ')">Adicionar</button>';
}

function confirmarAdicionarExercicio(diaIndex){
  const nomeEscolhido = document.getElementById('add-exercicio-select-' + diaIndex).value;
  const a = alunaAberta;
  a.treinoAtual.dias[diaIndex].ex.push(nomeEscolhido + ' · 3x12');
  if(typeof dias !== 'undefined' && dias[diaIndex]) dias[diaIndex].ex.push(nomeEscolhido + ' · 3x12');
  sincronizarTreinoComSupabase(a);
  atualizarDiaPersonalNaTela(diaIndex);
}

function adicionarExercicioTreino(diaIndex){
  const a = alunaAberta;
  a.treinoAtual.dias[diaIndex].ex.push('Novo exercício · 3x12');
  if(typeof dias !== 'undefined' && dias[diaIndex]) dias[diaIndex].ex.push('Novo exercício · 3x12');
  sincronizarTreinoComSupabase(a);
  atualizarDiaPersonalNaTela(diaIndex);
}

function abrirSubstituicao(diaIndex, exIndex){
  const container = document.getElementById('sub-picker-' + diaIndex + '-' + exIndex);
  if(!container) return;
  if(container.style.display !== 'none' && container.innerHTML){
    container.style.display = 'none';
    return;
  }

  const a = alunaAberta;
  const linhaAtual = a.treinoAtual.dias[diaIndex].ex[exIndex];
  const nomeAtual = linhaAtual.split(' · ')[0];

  // Regra: antes de oferecer troca, checa se ainda existe espaço pra progressão de carga nesse exercício
  const prog = getProgressoAluna(a.nome);
  const historicoDoExercicio = prog.historico[nomeAtual];
  const ultimoRegistro = historicoDoExercicio && historicoDoExercicio.length ? historicoDoExercicio[historicoDoExercicio.length - 1] : null;
  const aindaProgredindo = ultimoRegistro && ultimoRegistro.sugestao && ultimoRegistro.sugestao.texto === 'Aumentar carga';

  if(aindaProgredindo){
    container.innerHTML = '<div class="info-box" style="margin:8px 0;">' +
      '<p class="txt" style="color:var(--gold-soft);font-weight:600;margin-bottom:4px;">Ainda tem espaço pra evoluir nesse exercício</p>' +
      '<p class="txt" style="font-size:11px;">Na última sessão registrada, ela conseguiu aumentar a carga. Trocar agora interromperia essa progressão sem necessidade.</p>' +
      '<p style="font-size:11px;color:var(--gold-soft);margin-top:8px;cursor:pointer;" onclick="mostrarOpcoesDeTrocaMesmoAssim(' + diaIndex + ',' + exIndex + ')">Trocar mesmo assim (por outro motivo técnico/clínico)</p>' +
      '</div>';
    container.style.display = 'block';
    return;
  }

  mostrarOpcoesDeTrocaMesmoAssim(diaIndex, exIndex);
}

function mostrarOpcoesDeTrocaMesmoAssim(diaIndex, exIndex){
  const container = document.getElementById('sub-picker-' + diaIndex + '-' + exIndex);
  const a = alunaAberta;
  const linhaAtual = a.treinoAtual.dias[diaIndex].ex[exIndex];
  const nomeAtual = linhaAtual.split(' · ')[0];
  const exAtualBanco = exerciciosBanco.find(function(e){ return e.nome.toUpperCase() === nomeAtual.toUpperCase(); });
  const grupo = exAtualBanco ? (exAtualBanco.grupo || exAtualBanco.categoria) : null;
  const familiaAtual = obterFamiliaBiomecanica(nomeAtual);

  let opcoes = exerciciosBanco.filter(function(e){
    const grupoDoEx = e.grupo || e.categoria;
    return (!grupo || grupoDoEx === grupo) && e.nome.toUpperCase() !== nomeAtual.toUpperCase();
  });

  // Motor 5: prioriza mesma família biomecânica primeiro (troca com justificativa funcional, não aleatória)
  opcoes.sort(function(x, y){
    const xMesmaFamilia = familiaAtual && obterFamiliaBiomecanica(x.nome) === familiaAtual;
    const yMesmaFamilia = familiaAtual && obterFamiliaBiomecanica(y.nome) === familiaAtual;
    if(xMesmaFamilia && !yMesmaFamilia) return -1;
    if(!xMesmaFamilia && yMesmaFamilia) return 1;
    return 0;
  });
  window.__opcoesTrocaAtual = opcoes;
  window.__contextoTrocaAtual = { diaIndex: diaIndex, exIndex: exIndex, familiaAtual: familiaAtual };

  container.innerHTML = '<p style="font-size:11px;color:var(--text-faint);margin:8px 0 4px;">Trocar "' + nomeAtual + '" por (' + opcoes.length + ' opções):</p>' +
    '<input class="form-input" style="margin-bottom:6px;padding:8px 10px;font-size:12px;" placeholder="Buscar exercício..." oninput="filtrarOpcoesTroca(this.value)">' +
    '<div id="lista-opcoes-troca" style="display:flex;flex-direction:column;gap:5px;max-height:280px;overflow-y:auto;"></div>';
  container.style.display = 'block';
  renderizarOpcoesTrocaFiltradas(opcoes);
}

function abrirVideoApenasExercicio(nomeExercicio, ev){
  if(ev) ev.stopPropagation(); // nunca deixa o clique no nome também disparar a troca do exercício
  const exBanco = buscarExercicioNoBanco(nomeExercicio);
  const embed = exBanco ? getEmbedUrl(exBanco.video) : '';
  const overlay = document.createElement('div');
  overlay.id = 'overlay-video-apenas';
  overlay.style.cssText = 'position:fixed;inset:0;background:rgba(0,0,0,0.92);z-index:999999;display:flex;flex-direction:column;align-items:center;justify-content:center;padding:20px;';
  overlay.innerHTML = (embed
    ? '<div class="video-block" style="width:100%;max-width:400px;margin-bottom:16px;"><iframe src="' + embed + '" title="' + nomeExercicio + '" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></div>'
    : '<p style="color:#fff;font-size:13px;margin-bottom:16px;text-align:center;">Vídeo ainda não disponível pra esse exercício.</p>') +
    '<button class="btn-gold" style="width:auto;padding:10px 24px;margin:0;" onclick="document.getElementById(\'overlay-video-apenas\').remove()">Fechar</button>';
  document.body.appendChild(overlay);
}

function renderizarOpcoesTrocaFiltradas(opcoes){
  const lista = document.getElementById('lista-opcoes-troca');
  if(!lista) return;
  const ctx = window.__contextoTrocaAtual;
  lista.innerHTML = opcoes.map(function(e){
    const mesmaFamilia = ctx.familiaAtual && obterFamiliaBiomecanica(e.nome) === ctx.familiaAtual;
    return '<div style="background:var(--card-2);border:1px solid var(--border);border-radius:10px;padding:8px 10px;cursor:pointer;font-size:12px;display:flex;justify-content:space-between;align-items:center;gap:8px;" onclick="confirmarSubstituicao(' + ctx.diaIndex + ',' + ctx.exIndex + ',\'' + e.nome.replace(/'/g,"\\'") + '\')">' +
      '<span style="text-decoration:underline;text-decoration-color:var(--gold-soft);text-underline-offset:2px;" onclick="abrirVideoApenasExercicio(\'' + e.nome.replace(/'/g,"\\'") + '\', event)">' + e.nome + '</span>' +
      (mesmaFamilia ? '<span class="tag" style="background:var(--success-soft);color:var(--success);font-size:9px;flex-shrink:0;">mesmo padrão</span>' : '') +
    '</div>';
  }).join('') || '<p class="txt" style="font-size:11px;color:var(--text-faint);">Nenhum exercício encontrado com esse nome.</p>';
}

function filtrarOpcoesTroca(termo){
  const termoUpper = termo.trim().toUpperCase();
  const filtradas = !termoUpper ? window.__opcoesTrocaAtual : window.__opcoesTrocaAtual.filter(function(e){ return e.nome.toUpperCase().indexOf(termoUpper) !== -1; });
  renderizarOpcoesTrocaFiltradas(filtradas);
}

function confirmarSubstituicao(diaIndex, exIndex, novoNome){
  const a = alunaAberta;
  const linhaAtual = a.treinoAtual.dias[diaIndex].ex[exIndex];
  const nomeAntigo = linhaAtual.split(' · ')[0];
  const setsReps = linhaAtual.split(' · ')[1] || '';

  a.treinoAtual.dias[diaIndex].ex[exIndex] = novoNome + ' · ' + setsReps;
  if(typeof dias !== 'undefined' && dias[diaIndex] && dias[diaIndex].ex[exIndex]){
    dias[diaIndex].ex[exIndex] = novoNome + ' · ' + setsReps;
  }

  const prog = getProgressoAluna(a.nome);
  prog.substituicoes.push({ semana: prog.semana, de: nomeAntigo, para: novoNome, motivo: null });

  sincronizarTreinoComSupabase(a);
  salvarProgressoNoSupabase(a.nome);
  atualizarDiaPersonalNaTela(diaIndex);
}

function calcularContagemRegressivaAvaliacao(a){
  if(!a.dataAnamnese) return null;
  const dataBase = new Date(a.dataAnamnese + 'T00:00:00');
  const hoje = new Date();
  const dataProxima = new Date(dataBase.getTime() + 90*24*60*60*1000);
  const diasRestantes = Math.ceil((dataProxima - hoje) / (1000*60*60*24));
  return { diasRestantes: diasRestantes, atrasada: diasRestantes < 0 };
}

function calcularNivelEMotivo(a){
  const nivel = a.nivel || 'A definir';
  let motivo = '';
  const restricaoBaixa = a.restricoes && a.restricoes.toLowerCase().indexOf('nenhuma') === -1;
  if(nivel === 'Iniciante'){
    motivo = 'Iniciante: sem histórico de treino contínuo recente, ou tempo insuficiente de prática consolidada. Volume de base baixo (teto de 3 séries por exercício), foco em técnica antes de progressão de carga.';
  } else if(nivel === 'Intermediário'){
    motivo = 'Intermediário: já tem alguma consistência de treino, mas ainda não no ponto de tolerar o volume mais alto do avançado. Teto de 4 séries por exercício, progressão de carga já ativa desde o início.';
  } else if(nivel === 'Avançado'){
    motivo = 'Avançado: histórico de treino consolidado o suficiente pra sustentar volume alto (teto de 5 séries por exercício) e blocos de choque/periodização completa.';
  } else {
    motivo = 'Nível ainda não definido, revise antes de gerar o treino.';
  }
  if(restricaoBaixa) motivo += ' Atenção: ela relatou restrição/lesão, isso pode justificar um nível mais conservador mesmo com experiência de treino.';
  return { nivel: nivel, motivo: motivo };
}

function calcularStatusIMC(a){
  if(!a.imc) return null;
  const imcNum = parseFloat(String(a.imc).replace(',', '.'));
  if(isNaN(imcNum)) return null;
  if(imcNum >= 25){
    return { elevado: true, texto: 'IMC ' + a.imc + ' (≥25) → aciona Fase Emagrecimento por override automático da nossa metodologia, independente do objetivo declarado.' };
  }
  return { elevado: false, texto: 'IMC ' + a.imc + ' (dentro da faixa normal) → fase decidida pelo objetivo declarado, sem override.' };
}

function objetivoSugereTabata(a){
  const texto = ((a.objetivo || '') + ' ' + (a.piramide || '')).toLowerCase();
  const palavrasChave = ['emagrec', 'definir', 'definição', 'perder peso', 'perdi peso', 'perder gordura', 'secar', 'queimar gordura', 'redução de medida', 'peso', 'gordura'];
  return palavrasChave.some(function(p){ return texto.indexOf(p) !== -1; });
}

function responderSugestaoTabata(nomeAluna, quer){
  const a = alunasPersonal.find(function(x){ return x.nome === nomeAluna; });
  if(!a) return;
  a.tabataSugestaoRespondida = true;
  salvarPerfilAlunaNoSupabase(nomeAluna);
  const i = alunasPersonal.indexOf(a);
  openAlunaDetail(i);
  if(quer){
    setTimeout(function(){
      alert('Beleza! Desce até "Treino atual", abre o dia que quiser usar, e clica em "Transformar esse dia em treino Tabata de casa".');
    }, 100);
  }
}

function gerarResumoAnamnese(a){
  const partes = [];
  if(a.idade) partes.push(a.idade + ' anos');
  if(a.piramide){
    const piramideInfo = extrairEnfaseSecundaria(a.piramide);
    partes.push('prioriza ' + piramideInfo.enfase.toLowerCase() + (piramideInfo.secundario ? ' e depois ' + piramideInfo.secundario.toLowerCase() : ''));
  }
  if(a.objetivo) partes.push('objetivo: "' + (a.objetivo.length > 90 ? a.objetivo.slice(0,90) + '...' : a.objetivo) + '"');
  if(a.restricoes && a.restricoes.toLowerCase().indexOf('nenhuma') === -1) partes.push('atenção: ' + a.restricoes);
  if(a.academia) partes.push('treina na ' + a.academia);
  if(a.desviosPosturaisConfirmados && a.desviosPosturaisConfirmados.length){
    partes.push(a.desviosPosturaisConfirmados.length + ' desvio(s) postural(is) confirmado(s)');
  }
  if(partes.length === 0) return 'Anamnese ainda incompleta pra gerar um resumo.';
  return partes.join(' · ') + '.';
}

const descricoesMetodo = {
  'Restpause': 'Vai até perto da falha, descansa 15-20s, faz mais algumas reps com a mesma carga. Aumenta a intensidade sem aumentar muito o tempo.',
  'Dropset': 'Chega na falha, reduz a carga na hora (sem descanso) e continua até falhar de novo. Ótimo pra fechar um exercício isolado.',
  'Cluster set': 'Divide a série em blocos pequenos com descanso curto entre eles, permite mover mais carga total. Usado no bloco de choque.',
  'Bi-set': 'Dois exercícios seguidos, sem descanso entre eles, só depois do segundo. Economiza tempo e aumenta o estímulo metabólico.',
  'Tri-set': 'Três exercícios seguidos sem descanso. Mais intenso que o bi-set, usar com cuidado no volume total do dia.',
  'Pirâmide crescente': 'Aumenta a carga e reduz as reps a cada série. Bom pra quem já tem técnica consolidada.'
};

// ===== MOTOR 6: MÉTODOS — decide automaticamente quais métodos fazem sentido oferecer agora =====
// Nunca libera método avançado pra quem não tem base técnica/nível pra isso.
function metodosPermitidosAgora(a){
  const classificacao = classificarAluna(a);
  const faseInfo = determinarFaseMacrociclo(a);

  // Deload: nunca combina métodos, é semana de recuperação
  if(faseInfo.blocoTecnico === 'deload') return ['Nenhum'];

  // Recuperação comprometida: só o básico, não empilha mais fadiga
  if(classificacao.capacidadeRecuperacao === 'reduzida') return ['Nenhum', 'Bi-set'];

  if(classificacao.nivel === 'Iniciante' || classificacao.experienciaTecnica === 'iniciante'){
    return ['Nenhum']; // iniciante foca em técnica e progressão de carga simples primeiro
  }

  if(classificacao.nivel === 'Intermediário'){
    return ['Nenhum', 'Bi-set', 'Restpause'];
  }

  // Avançado: libera tudo, mas Cluster/Tri-set só fazem sentido de verdade no bloco de choque
  if(faseInfo.blocoTecnico === 'choque'){
    return ['Nenhum', 'Bi-set', 'Restpause', 'Dropset', 'Cluster set', 'Tri-set', 'Pirâmide crescente'];
  }
  return ['Nenhum', 'Bi-set', 'Restpause', 'Dropset', 'Pirâmide crescente'];
}

async function abrirTreinosArquivados(nomeAluna){
  const a = alunasPersonal.find(function(x){ return x.nome === nomeAluna; });
  const area = document.getElementById('treinos-arquivados-area');
  if(!a || !area) return;

  // Funciona como liga/desliga: clicar de novo fecha, sem precisar buscar tudo outra vez
  if(area.innerHTML){ area.innerHTML = ''; return; }

  if(!supabaseClient || !a.email){
    area.innerHTML = '<div class="info-box"><p class="txt">Sem conexão com o histórico agora, ou ela ainda não tem e-mail cadastrado.</p></div>';
    return;
  }

  area.innerHTML = '<p class="txt" style="color:var(--text-faint);">Carregando histórico...</p>';

  try {
    const { data: alunaRow } = await supabaseClient.from('alunas').select('auth_id').eq('email', a.email).maybeSingle();
    if(!alunaRow || !alunaRow.auth_id){
      area.innerHTML = '<div class="info-box"><p class="txt">Ela ainda não tem login criado, então ainda não existe histórico salvo no banco.</p></div>';
      return;
    }
    const { data: historico } = await supabaseClient.from('treinos').select('*').eq('aluna_id', alunaRow.auth_id).order('updated_at', { ascending: false });

    if(!historico || historico.length === 0){
      area.innerHTML = '<div class="info-box"><p class="txt">Nenhum treino arquivado ainda.</p></div>';
      return;
    }

    area.innerHTML = '<p class="txt" style="color:var(--gold-soft);cursor:pointer;margin-bottom:8px;" onclick="document.getElementById(\'treinos-arquivados-area\').innerHTML=\'\';"><i class="ti ti-x" style="font-size:11px;margin-right:4px;"></i>Fechar histórico</p>' +
      historico.map(function(t, idx){
      const data = new Date(t.updated_at).toLocaleDateString('pt-BR');
      const rotulo = idx === 0 ? 'Atual' : 'Arquivado';
      return '<div class="list-item" style="flex-direction:column;align-items:stretch;">' +
        '<div style="display:flex;justify-content:space-between;"><span>' + data + '</span><span class="tag">' + rotulo + ' · ' + t.fase + '</span></div>' +
        '<p style="font-size:11px;color:var(--text-faint);margin:4px 0 0;">' + t.dias.length + ' dias de treino</p>' +
      '</div>';
    }).join('');
  } catch(erroDeRede){
    area.innerHTML = '<div class="info-box"><p class="txt">Sem conexão com o histórico agora, tenta de novo.</p></div>';
  }
}

function regerarTabataParaAluna(nomeAluna, di){
  const a = alunasPersonal.find(function(x){ return x.nome === nomeAluna; });
  if(!a || !a.treinoAtual || !a.treinoAtual.dias[di]) return;
  const nomeDia = a.treinoAtual.dias[di].n;
  const tabataGerado = gerarTreinoTabata(a.nivel, a.restricoes);
  tabataGerado.n = nomeDia;
  tabataGerado.foco = 'Tabata de casa · ' + a.nivel;
  a.treinoAtual.dias[di] = tabataGerado;
  sincronizarTreinoComSupabase(a);
  atualizarDiaPersonalNaTela(di);
}

function removerTabataDoDia(nomeAluna, di){
  const a = alunasPersonal.find(function(x){ return x.nome === nomeAluna; });
  if(!a || !a.treinoAtual || !a.treinoAtual.dias[di]) return;
  const nomeDia = a.treinoAtual.dias[di].n;
  a.treinoAtual.dias[di] = { n: nomeDia, foco: 'Descanso', descanso: true, ex: [] };
  sincronizarTreinoComSupabase(a);
  atualizarDiaPersonalNaTela(di);
}

function renderConteudoDiaPersonal(a, di){
  const d = a.treinoAtual.dias[di];
  let html = '';
  if(d.tabata){
    html += '<div class="info-box" style="margin-bottom:8px;">' +
      '<p class="lbl">Tabata · ' + d.nivel + '</p>' +
      '<p class="txt">' + d.trabalhoSeg + 's trabalho / ' + d.descansoSeg + 's descanso · ' + d.ciclos + ' ciclos · ' + d.blocos.length + ' bloco(s) · ~' + d.duracaoTotalMin + ' min</p>' +
      '<p class="txt" style="color:var(--text-faint);font-size:11px;">' + d.descricaoProtocolo + '</p>' +
    '</div>';
    d.blocos.forEach(function(bloco, bi){
      html += '<div class="list-item"><span>Bloco ' + (bi+1) + ': ' + bloco.nome + '</span></div>';
    });
    html += '<p style="font-size:12px;color:var(--gold-soft);margin:6px 0 4px;cursor:pointer;" onclick="regerarTabataParaAluna(\'' + a.nome.replace(/'/g,"\\'") + '\',' + di + ')"><i class="ti ti-refresh" style="font-size:12px;vertical-align:-2px;margin-right:4px;"></i>Gerar outra combinação</p>' +
      '<p style="font-size:12px;color:var(--text-faint);margin:2px 0 4px;cursor:pointer;" onclick="removerTabataDoDia(\'' + a.nome.replace(/'/g,"\\'") + '\',' + di + ')">Voltar esse dia pro treino normal</p>';
  } else {
    d.ex.forEach(function(exLine, ei){
      const partesEdit = exLine.split(' · ');
      const nomeEdit = partesEdit[0];
      const setsRepsEdit = partesEdit[1] || '';
      const metodoAplicado = d.metodos && d.metodos[ei];
      html += '<div class="list-item exercicio-linha" data-dia="' + di + '" data-ex="' + ei + '" style="flex-direction:column;align-items:stretch;gap:4px;padding:8px 12px;">' +
        '<div style="display:flex;justify-content:space-between;align-items:center;">' +
          '<div style="display:flex;align-items:center;gap:8px;min-width:0;">' +
            '<span class="alca-arrastar" data-dia="' + di + '" data-ex="' + ei + '" onpointerdown="iniciarArrastarExercicio(event)" style="cursor:grab;color:var(--text-faint);touch-action:none;flex-shrink:0;"><i class="ti ti-grip-vertical" style="font-size:16px;"></i></span>' +
            '<span id="nome-ex-' + di + '-' + ei + '" style="font-size:12px;cursor:pointer;" onclick="abrirSubstituicao(' + di + ',' + ei + ')">' + nomeEdit + (metodoAplicado ? ' <span class="tag" style="background:var(--gold-soft);color:#1A1409;">' + metodoAplicado + '</span>' : '') + '</span>' +
          '</div>' +
          '<span style="display:flex;gap:4px;align-items:center;flex-shrink:0;">' +
            '<span class="acao-pill" onclick="alternarMetodoExercicio(' + di + ',' + ei + ')">Método</span>' +
            '<span class="acao-pill" onclick="abrirSubstituicao(' + di + ',' + ei + ')">Trocar</span>' +
          '</span>' +
        '</div>' +
        '<div id="metodo-picker-' + di + '-' + ei + '" style="display:none;"></div>' +
        '<div id="sub-picker-' + di + '-' + ei + '" style="display:none;"></div>' +
        '<div style="display:flex;gap:6px;">' +
          '<input class="form-input" style="flex:1;padding:6px 8px;font-size:12px;" value="' + setsRepsEdit + '" onchange="editarSeriesReps(' + di + ',' + ei + ',this.value)" placeholder="ex: 4x12">' +
          '<button class="acao-pill destrutiva" style="border-radius:8px;" onclick="removerExercicioTreino(' + di + ',' + ei + ')">Remover</button>' +
        '</div>' +
      '</div>';
    });
    html += '<p style="font-size:12px;color:var(--gold-soft);margin:6px 0 4px;cursor:pointer;" onclick="abrirSelecaoGrupoParaAdicionar(' + di + ')"><i class="ti ti-plus" style="font-size:12px;vertical-align:-2px;margin-right:4px;"></i>Adicionar exercício nesse dia</p>' +
      '<div id="add-exercicio-form-' + di + '" style="margin-bottom:6px;"></div>' +
      '<p style="font-size:11px;color:var(--text-faint);margin:6px 0 4px;cursor:pointer;" onclick="regerarTabataParaAluna(\'' + a.nome.replace(/'/g,"\\'") + '\',' + di + ')">Transformar esse dia em treino Tabata de casa</p>';
  }
  return html;
}

function atualizarDiaPersonalNaTela(di){
  const container = document.getElementById('dia-personal-expandido-' + di);
  if(!container || !alunaAberta) return;
  container.innerHTML = renderConteudoDiaPersonal(alunaAberta, di);
}

function alternarDiaExpandidoPersonal(di){
  const container = document.getElementById('dia-personal-expandido-' + di);
  if(!container) return;
  container.style.display = container.style.display === 'none' ? 'block' : 'none';
}

function alternarMetodoExercicio(di, ei){
  const container = document.getElementById('metodo-picker-' + di + '-' + ei);
  if(!container) return;
  if(container.style.display === 'none' || !container.innerHTML){
    const metodos = metodosPermitidosAgora(alunaAberta);
    container.innerHTML = '<div style="display:flex;flex-direction:column;gap:6px;margin:8px 0;">' +
      metodos.map(function(m){
        return '<div style="background:var(--card-2);border:1px solid var(--border);border-radius:12px;padding:10px 12px;cursor:pointer;" onclick="aplicarMetodoExercicio(' + di + ',' + ei + ',\'' + m + '\')">' +
          '<p style="font-size:12.5px;font-weight:700;color:var(--gold-soft);margin:0 0 3px;">' + m + '</p>' +
          (descricoesMetodo[m] ? '<p style="font-size:11px;color:var(--text-faint);margin:0;">' + descricoesMetodo[m] + '</p>' : '') +
        '</div>';
      }).join('') +
    '</div>';
    container.style.display = 'block';
  } else {
    container.style.display = 'none';
  }
}

function aplicarMetodoExercicio(di, ei, metodo){
  const a = alunaAberta;
  if(!a.treinoAtual.dias[di].metodos) a.treinoAtual.dias[di].metodos = {};
  if(metodo === 'Nenhum') delete a.treinoAtual.dias[di].metodos[ei];
  else a.treinoAtual.dias[di].metodos[ei] = metodo;
  sincronizarTreinoComSupabase(a);

  // Atualiza só esse exercício na tela, sem recarregar a ficha (isso fechava o dia e o picker)
  const nomeEx = a.treinoAtual.dias[di].ex[ei].split(' · ')[0];
  const elNome = document.getElementById('nome-ex-' + di + '-' + ei);
  if(elNome){
    elNome.innerHTML = nomeEx + (metodo !== 'Nenhum' ? ' <span class="tag" style="background:var(--gold-soft);color:#1A1409;">' + metodo + '</span>' : '');
  }
  const picker = document.getElementById('metodo-picker-' + di + '-' + ei);
  if(picker){ picker.style.display = 'none'; picker.innerHTML = ''; }
}

let treinoJaBuscadoPara = {};
async function buscarTreinoRealDaAluna(a, i){
  if(!supabaseClient || !a.email) return;
  const chaveCache = a.email.toLowerCase();
  if(treinoJaBuscadoPara[chaveCache]) return; // já buscou nessa sessão, não repete à toa toda vez que reabre
  try {
    const { data: alunaRow } = await supabaseClient.from('alunas').select('auth_id, treino_atual_backup').eq('email', a.email).maybeSingle();
    treinoJaBuscadoPara[chaveCache] = true;
    if(!alunaRow) return;

    if(alunaRow.auth_id){
      const { data: treinoData } = await supabaseClient.from('treinos').select('*').eq('aluna_id', alunaRow.auth_id).order('updated_at', { ascending: false }).limit(1).maybeSingle();
      if(treinoData){
        a.treinoAtual = { fase: treinoData.fase, volume: treinoData.volume, dias: treinoData.dias };
        if(alunaAberta === a) openAlunaDetail(i);
      } else {
        const statusEl = document.getElementById('treino-status-inicial');
        if(statusEl) statusEl.textContent = 'Confirmado: ela ainda não tem nenhum treino gerado.';
      }
    } else if(alunaRow.treino_atual_backup){
      a.treinoAtual = alunaRow.treino_atual_backup;
      if(alunaAberta === a) openAlunaDetail(i);
    } else {
      const statusEl = document.getElementById('treino-status-inicial');
      if(statusEl) statusEl.textContent = 'Confirmado: ela ainda não tem nenhum treino gerado.';
    }
  } catch(erroDeRede){
    console.warn('Sem conexão pra buscar o treino real dela agora, mostrando o que já está localmente:', erroDeRede);
  }
}

function abrirResumoCompletoAluna(nomeAluna){
  const a = alunasPersonal.find(function(x){ return x.nome === nomeAluna; });
  if(!a) return;
  showPersonalView('resumo-aluna');

  let queixaHtml = '';
  if(a.queixaDor && !a.patologiaConfirmada){
    const candidatas = patologiasCatalogo.filter(function(p){ return p.regiao === a.regiaoQueixa; });
    queixaHtml = '<p class="section-label">IA identificou uma queixa</p>' +
      '<div class="insight"><p>A aluna relatou dor ("' + a.restricoes + '"). Isso pode ser só ajuste de técnica, mas também pode indicar uma das patologias abaixo. Confirme se for o caso:</p></div>' +
      '<div class="chip-list" style="margin-bottom:14px;">' +
        candidatas.map(function(p){ return '<span class="desvio-chip" onclick="confirmarPatologia(\'' + a.nome.replace(/'/g,"\\'") + '\',\'' + p.id + '\')">' + p.nome + '</span>'; }).join('') +
        '<span class="desvio-chip" onclick="confirmarPatologia(\'' + a.nome.replace(/'/g,"\\'") + '\',\'tecnica\')">Não é patologia, só ajuste de técnica</span>' +
      '</div>';
  } else if(a.patologiaConfirmada === 'tecnica'){
    queixaHtml = '<p class="section-label">Queixa avaliada</p>' +
      '<div class="insight"><p>Confirmado como ajuste de técnica, não patologia. Exercícios podem voltar à prescrição normal quando o personal achar adequado.</p></div>';
  } else if(a.patologiaConfirmada){
    const p = patologiasCatalogo.find(function(x){ return x.id === a.patologiaConfirmada; });
    queixaHtml = '<p class="section-label">Patologia confirmada</p>' +
      '<div class="info-box"><p style="font-size:13px;font-weight:600;margin:0 0 8px;">' + p.nome + '</p>' +
      '<p class="txt"><b>Evitar:</b> ' + p.evitar + '</p>' +
      '<p class="txt"><b>Permitido:</b> ' + p.permitido + '</p>' +
      '<p class="txt" style="margin-bottom:0;"><b>Conduta:</b> ' + p.conduta + '</p></div>';
  }

  const nivelInfo = calcularNivelEMotivo(a);
  const imcInfo = calcularStatusIMC(a);
  const contagem = calcularContagemRegressivaAvaliacao(a);
  let htmlDiagnostico = '<div class="info-box"><p class="txt" style="margin-bottom:0;">' + gerarResumoAnamnese(a) + '</p></div>' +
    '<div class="insight" style="margin-top:10px;"><p><b>Nível sugerido pela metodologia: ' + nivelInfo.nivel + '</b><br>' + nivelInfo.motivo + '</p>' +
    (imcInfo ? '<p style="margin-top:8px;' + (imcInfo.elevado ? 'color:#E2A33D;' : '') + '">' + imcInfo.texto + '</p>' : '') +
    '</div>';
  if(contagem){
    const cor = contagem.atrasada ? '#E2A33D' : (contagem.diasRestantes <= 15 ? 'var(--gold-soft)' : 'var(--text-dim)');
    const texto = contagem.atrasada ? 'Avaliação física atrasada há ' + Math.abs(contagem.diasRestantes) + ' dias' : contagem.diasRestantes + ' dias pra próxima avaliação física sugerida';
    htmlDiagnostico += '<div class="list-item" style="margin-top:8px;"><span><i class="ti ti-calendar-time" style="color:' + cor + ';margin-right:8px;"></i>' + texto + '</span></div>';
  }
  if(a.imc) htmlDiagnostico += '<p class="section-label" style="margin-top:14px;">Composição corporal</p><div class="info-box"><p class="txt">' + a.peso + ' · ' + a.altura + '<br>IMC ' + a.imc + '</p></div>';
  htmlDiagnostico += renderDirecionamentoTecnico(a) + renderResumoMetodologiaAutomatica(a) + renderDuvidasSinalizadas(a) + renderExerciciosEstagnados(a);

  document.getElementById('resumo-aluna-content').innerHTML =
    '<h1 class="page-title" style="margin-top:0;">Resumo completo</h1>' +
    '<p class="page-sub" style="margin-top:-6px;">' + a.nome + '</p>' +
    renderSecaoColapsavel('Diagnóstico da metodologia', htmlDiagnostico, 'resumocompleto-' + a.nome.replace(/[^a-zA-Z0-9]/g,'')) +
    renderAvaliacaoPostural(a) +
    renderSecaoColapsavel('Pirâmide de prioridade (resposta original)', '<div class="info-box"><p class="txt">' + (a.piramide || 'Não respondida') + '</p></div>', 'piramideorig-' + a.nome.replace(/[^a-zA-Z0-9]/g,'')) +
    renderSecaoColapsavel('Objetivo com a consultoria', '<div class="info-box"><p class="txt">' + (a.objetivo || 'Não informado') + '</p></div>', 'objetivo-' + a.nome.replace(/[^a-zA-Z0-9]/g,'')) +
    renderSecaoColapsavel('Restrições / lesões relatadas', '<div class="info-box"><p class="txt">' + a.restricoes + '</p></div>' + queixaHtml, 'restricoes-' + a.nome.replace(/[^a-zA-Z0-9]/g,'')) +
    renderSecaoColapsavel('Academia', '<div class="info-box"><p class="txt">' + (a.academia || 'Não informado') + '</p></div>', 'academia-' + a.nome.replace(/[^a-zA-Z0-9]/g,'')) +
    renderSecaoColapsavel('Plano fechado', renderPlanoFechadoConteudo(a), 'planofechado-' + a.nome.replace(/[^a-zA-Z0-9]/g,'')) +
    renderSecaoColapsavel('Roda da vida', renderRodaDaVidaNaFicha(a.nome), 'rodadavida-' + a.nome.replace(/[^a-zA-Z0-9]/g,''));
}

function openAlunaDetail(i){
  const a = alunasPersonal[i];
  alunaAberta = a;
  const el = document.getElementById('aluna-detail-content');
  buscarTreinoRealDaAluna(a, i);

  let treinoHtml = '<div class="list-item"><span id="treino-status-inicial">Verificando se já existe um treino salvo pra ela...</span></div>';
  let treinoAcoesHtml = '';
  if(a.treinoAtual){
    treinoHtml = '<div class="badge">' + a.treinoAtual.fase + '</div>' +
      '<p class="page-sub" style="margin:2px 0 10px;">' + a.treinoAtual.volume + '</p>';
    a.treinoAtual.dias.forEach(function(d, di){
      const letraMatch = d.foco.match(/^Treino (\w)/);
      const letra = letraMatch ? letraMatch[1] : (di + 1);
      treinoHtml += '<div style="border:1px solid var(--border);border-radius:14px;margin-bottom:10px;overflow:hidden;">' +
        '<div style="display:flex;align-items:center;gap:12px;padding:12px;cursor:pointer;" onclick="alternarDiaExpandidoPersonal(' + di + ')">' +
          '<div style="width:36px;height:36px;border-radius:10px;background:linear-gradient(135deg,#F4D9A5,#E8C58A);display:flex;align-items:center;justify-content:center;flex-shrink:0;"><span style="font-family:\'Playfair Display\',serif;font-size:16px;font-weight:700;color:#1A1409;">' + letra + '</span></div>' +
          '<div style="flex:1;"><p style="font-size:13px;font-weight:600;margin:0;">' + d.n + '</p><p style="font-size:11px;color:var(--text-faint);margin:0;">' + (d.foco.indexOf(' · ') !== -1 ? d.foco.split(' · ').slice(1).join(' · ') : d.foco) + '</p></div>' +
          '<i class="ti ti-chevron-down" style="color:var(--text-faint);"></i>' +
        '</div>' +
        '<div id="dia-personal-expandido-' + di + '" style="display:none;padding:0 12px 12px;">' + renderConteudoDiaPersonal(a, di) + '</div></div>';
    });
  }
  const diagnostico = gerarDiagnosticoInterno(a);
  const prog = getProgressoAluna(a.nome);
  if(a.treinoAtual){
    treinoAcoesHtml = '<button class="btn-gold" style="background:var(--card-2);color:var(--gold-soft);border:1px solid var(--border);margin-top:10px;" onclick="mostrarDiagnosticoParaProgredir(\'' + a.nome.replace(/'/g,"\\'") + '\')"><i class="ti ti-trending-up" style="font-size:13px;vertical-align:-2px;margin-right:6px;"></i>Progredir treino</button>';
    treinoAcoesHtml += '<div id="diagnostico-progressao-area"></div>';
  } else {
    treinoAcoesHtml = '<div class="info-box" style="margin-top:10px;">' +
      '<p class="lbl">Diagnóstico antes de gerar</p>' +
      '<p class="txt"><b>Estratégia agora:</b> ' + diagnostico.estrategiaAtual + '</p>' +
      '<p class="txt" style="font-size:11px;color:var(--text-faint);">' + diagnostico.justificativaEstrategia + '</p>' +
      '<p class="txt" style="margin-top:4px;"><b>Fase:</b> ' + diagnostico.faseAtual + '</p>' +
      '</div>';
    treinoAcoesHtml += '<button class="btn-gold" style="background:var(--card-2);color:var(--gold-soft);border:1px solid var(--border);margin-top:10px;" onclick="gerarTreinoAutomaticoParaAluna(\'' + a.nome.replace(/'/g,"\\'") + '\')"><i class="ti ti-wand" style="font-size:13px;vertical-align:-2px;margin-right:6px;"></i>Prescrever treino automaticamente</button>';
  }
  treinoAcoesHtml += '<div id="validacao-treino-area"></div>';
  treinoAcoesHtml += '<p class="section-label" style="margin-top:18px;">Seu feedback sobre esse treino</p>' +
    '<p class="page-sub" style="margin-top:-4px;">Anote aqui o que achou estranho ou errado, isso fica guardado organizado pra você trazer numa próxima conversa e a gente transformar em regra permanente.</p>' +
    '<div class="form-group"><textarea class="form-input" id="feedback-gerador-texto" rows="3" placeholder="Ex: achei o volume de posterior baixo demais pra essa aluna..."></textarea></div>' +
    '<button class="btn-gold" style="background:var(--card-2);color:var(--gold-soft);border:1px solid var(--border);" onclick="salvarFeedbackGerador(\'' + a.nome.replace(/'/g,"\\'") + '\')">Salvar feedback</button>' +
    '<div id="lista-feedback-gerador"></div>';

  let queixaHtml = '';
  if(a.queixaDor && !a.patologiaConfirmada){
    const candidatas = patologiasCatalogo.filter(function(p){ return p.regiao === a.regiaoQueixa; });
    queixaHtml = '<p class="section-label">IA identificou uma queixa</p>' +
      '<div class="insight"><p>A aluna relatou dor ("' + a.restricoes + '"). Isso pode ser só ajuste de técnica, mas também pode indicar uma das patologias abaixo. Confirme se for o caso:</p></div>' +
      '<div class="chip-list" style="margin-bottom:14px;">' +
        candidatas.map(function(p){ return '<span class="desvio-chip" onclick="confirmarPatologia(\'' + a.nome.replace(/'/g,"\\'") + '\',\'' + p.id + '\')">' + p.nome + '</span>'; }).join('') +
        '<span class="desvio-chip" onclick="confirmarPatologia(\'' + a.nome.replace(/'/g,"\\'") + '\',\'tecnica\')">Não é patologia, só ajuste de técnica</span>' +
      '</div>';
  } else if(a.patologiaConfirmada === 'tecnica'){
    queixaHtml = '<p class="section-label">Queixa avaliada</p>' +
      '<div class="insight"><p>Confirmado como ajuste de técnica, não patologia. Exercícios podem voltar à prescrição normal quando o personal achar adequado.</p></div>';
  } else if(a.patologiaConfirmada){
    const p = patologiasCatalogo.find(function(x){ return x.id === a.patologiaConfirmada; });
    queixaHtml = '<p class="section-label">Patologia confirmada</p>' +
      '<div class="info-box"><p style="font-size:13px;font-weight:600;margin:0 0 8px;">' + p.nome + '</p>' +
      '<p class="txt"><b>Evitar:</b> ' + p.evitar + '</p>' +
      '<p class="txt"><b>Permitido:</b> ' + p.permitido + '</p>' +
      '<p class="txt" style="margin-bottom:0;"><b>Conduta:</b> ' + p.conduta + '</p></div>';
  }

  el.innerHTML =
    '<div style="display:flex;justify-content:space-between;align-items:flex-start;">' +
      '<h1 class="page-title" style="margin-top:0;margin-bottom:2px;">' + a.nome + '</h1>' +
      '<div style="display:flex;gap:6px;margin-top:2px;">' +
        (a.telefone ? '<a href="https://wa.me/55' + a.telefone.replace(/\D/g,'') + '" target="_blank" rel="noopener" style="width:34px;height:34px;border-radius:50%;background:var(--card-2);border:1px solid var(--border);display:flex;align-items:center;justify-content:center;"><i class="ti ti-brand-whatsapp" style="color:var(--gold-soft);font-size:16px;"></i></a>' : '') +
        (a.email ? '<a href="mailto:' + a.email + '" style="width:34px;height:34px;border-radius:50%;background:var(--card-2);border:1px solid var(--border);display:flex;align-items:center;justify-content:center;"><i class="ti ti-mail" style="color:var(--gold-soft);font-size:16px;"></i></a>' : '') +
      '</div>' +
    '</div>' +
    '<p class="page-sub"><span class="status-dot ' + a.status + '"></span><span class="status-txt ' + a.status + '">' + a.statusLabel + '</span></p>' +
    '<div class="stat-grid">' +
      '<div class="stat-card"><p class="stat-label">Nível</p>' +
        '<select class="form-select" style="font-size:13px;padding:6px;margin-top:4px;" onchange="editarNivelAluna(\'' + a.nome.replace(/'/g,"\\'") + '\',this.value)">' +
          ['Iniciante','Intermediário','Avançado'].map(function(n){ return '<option value="' + n + '"' + (a.nivel === n ? ' selected' : '') + '>' + n + '</option>'; }).join('') +
        '</select></div>' +
      '<div class="stat-card"><p class="stat-label">Frequência desejada</p>' +
        '<select class="form-select" style="font-size:13px;padding:6px;margin-top:4px;" onchange="editarFrequenciaAluna(\'' + a.nome.replace(/'/g,"\\'") + '\',this.value)">' +
          ['2x por semana','3x por semana','4x por semana','5x por semana','6x por semana'].map(function(f){ return '<option value="' + f + '"' + (a.freq === f ? ' selected' : '') + '>' + f + '</option>'; }).join('') +
        '</select></div>' +
      '<div class="stat-card"><p class="stat-label">Treina em</p>' +
        '<select class="form-select" style="font-size:13px;padding:6px;margin-top:4px;" onchange="editarAmbienteTreinoAluna(\'' + a.nome.replace(/'/g,"\\'") + '\',this.value)">' +
          ['Academia','Casa'].map(function(amb){ return '<option value="' + amb + '"' + ((a.ambienteTreino || 'Academia') === amb ? ' selected' : '') + '>' + amb + '</option>'; }).join('') +
        '</select></div>' +
      '<div class="stat-card"><p class="stat-label">Data de nascimento</p>' +
        '<input type="date" class="form-input" style="font-size:13px;padding:6px;margin-top:4px;" value="' + (a.dataNascimento || '') + '" onchange="editarDataNascimentoAluna(\'' + a.nome.replace(/'/g,"\\'") + '\',this.value)"></div>' +
    '</div>' +
    '<p style="font-size:11px;color:var(--text-faint);margin:-8px 0 12px;">Ajustar aqui atualiza automaticamente toda a estrutura de treino gerada — o gerador nunca mistura exercício de academia com exercício de casa</p>' +
    '<div id="preview-mudanca-area"></div>' +
    '<button class="btn-gold" style="background:var(--card-2);color:var(--gold-soft);border:1px solid var(--border);margin-bottom:18px;" onclick="abrirResumoCompletoAluna(\'' + a.nome.replace(/'/g,"\\'") + '\')"><i class="ti ti-file-description" style="margin-right:6px;"></i>📋 Resumo completo (diagnóstico, avaliação, plano)</button>' +
    '<button class="btn-gold" style="background:var(--card-2);color:var(--gold-soft);border:1px solid var(--border);margin-bottom:8px;" onclick="gerarResumoPerformanceAluna(\'' + a.nome.replace(/'/g,"\\'") + '\')"><i class="ti ti-sparkles" style="margin-right:6px;"></i>🤖 Resumir performance com IA</button>' +
    '<div id="resumo-ia-area" style="margin-bottom:18px;"></div>' +
    (objetivoSugereTabata(a) && !a.tabataSugestaoRespondida ? (
      '<div class="info-box" style="margin-bottom:12px;border-color:var(--border-strong);">' +
        '<p class="lbl">💡 Sugestão baseada no objetivo dela</p>' +
        '<p class="txt">O objetivo relatado por ela menciona emagrecimento/definição. Um protocolo Tabata (circuito intervalado, ' + (a.nivel || 'Iniciante') + ') pode complementar bem o treino resistido nesse caso. Quer que eu já deixe pronto pra você aplicar num dos dias?</p>' +
        '<div style="display:flex;gap:8px;margin-top:8px;">' +
          '<span class="chip" style="cursor:pointer;background:var(--success-soft);color:var(--success);" onclick="responderSugestaoTabata(\'' + a.nome.replace(/'/g,"\\'") + '\',true)">Sim, quero ver</span>' +
          '<span class="chip" style="cursor:pointer;" onclick="responderSugestaoTabata(\'' + a.nome.replace(/'/g,"\\'") + '\',false)">Não, obrigado</span>' +
        '</div>' +
      '</div>'
    ) : '') +
    (detectarRiscoAbandonoPorConstancia(a.nome) ? (
      '<div class="info-box" style="margin-top:14px;border-color:#E2A33D;">' +
        '<p class="lbl" style="color:#E2A33D;">⚠ Risco de abandono</p>' +
        '<p class="txt">3 semanas seguidas com constância abaixo de 40%. O volume do próximo treino já foi reduzido automaticamente pra facilitar ela voltar. Vale puxar ela de volta com uma mensagem agora.</p>' +
        (a.telefone ? '<a href="https://wa.me/55' + a.telefone.replace(/\D/g,'') + '" target="_blank" rel="noopener" class="btn-gold" style="display:inline-block;text-decoration:none;text-align:center;margin-top:8px;padding:10px 16px;width:auto;"><i class="ti ti-brand-whatsapp" style="margin-right:6px;"></i>Chamar no WhatsApp</a>' : '') +
      '</div>'
    ) : '') +
    '<p class="section-label" style="margin-top:24px;">Treino atual</p>' +
    '<p style="font-size:12px;color:var(--gold-soft);margin:6px 0 12px;cursor:pointer;" onclick="abrirTreinosArquivados(\'' + a.nome.replace(/'/g,"\\'") + '\')"><i class="ti ti-archive" style="font-size:12px;vertical-align:-1px;margin-right:4px;"></i>Ver treinos arquivados (histórico)</p>' +
    '<div id="treinos-arquivados-area"></div>' +
    treinoHtml +
    treinoAcoesHtml +
    renderSecaoColapsavel('Acompanhamento e histórico', renderElegibilidadeFase(a) + renderFunilEngajamento(a) + renderLinhaDoTempo(a) + renderPromocaoNivel(a) + renderBlocoPeriodizacao(a) + renderTecnicaPendente(a) + renderProgressao(a), 'acompanhamento') +
    '<p class="section-label" style="margin-top:22px;">Acesso ao app</p>' +
    (a.authId && a.senhaGerada
      ? '<div class="info-box"><p class="lbl" style="color:var(--success);">✓ Acesso já criado automaticamente</p>' +
        '<p class="txt">E-mail: ' + a.email + '<br>Senha: <b>' + a.senhaGerada + '</b><br>Link do app: ' + (LINK_DO_APP) + '</p>' +
        '</div>' +
        '<button class="btn-gold" style="width:auto;padding:10px 16px;margin:8px 8px 0 0;font-size:13px;background:#25D366;color:#fff;border:none;" onclick="enviarCredenciaisPorWhatsApp(\'' + a.nome.replace(/'/g,"\\'") + '\')"><i class="ti ti-brand-whatsapp" style="vertical-align:-2px;margin-right:6px;"></i>Mandar login e senha por WhatsApp</button>' +
        '<button class="btn-gold" style="width:auto;padding:10px 16px;margin:8px 0 0;font-size:13px;background:var(--card-2);color:var(--gold-soft);border:1px solid var(--border);" onclick="navigator.clipboard.writeText(\'E-mail: ' + a.email + ' - Senha: ' + a.senhaGerada + ' - Link: ' + (LINK_DO_APP) + '\')">Copiar dados</button>'
      : '<button class="btn-gold" style="background:var(--card-2);color:var(--gold-soft);border:1px solid var(--border);" onclick="criarLoginParaAluna(\'' + a.nome.replace(/'/g,"\\'") + '\')"><i class="ti ti-key" style="font-size:14px;vertical-align:-2px;margin-right:6px;"></i>Gerar acesso ao app</button>'
    ) +
    '<div id="acesso-gerado-area"></div>' +
    '<p class="page-sub" style="margin-top:10px;">Anamnese respondida em ' + (a.dataAnamnese || 'data desconhecida') + '</p>';
  showPersonalView('aluna');
  aplicarTransicaoSuave('aluna-detail-content');
  renderListaFeedbackGerador(a.nome);
}

function renderPromocaoNivel(a){
  const elegibilidade = calcularElegibilidadePromocaoNivel(a.nome);
  if(!elegibilidade) return '';
  let html = '<p class="section-label" style="margin-top:22px;">Promoção de nível (iniciante → intermediário)</p>';
  html += elegibilidade.criterios.map(function(c){
    return '<div class="list-item"><span><i class="ti ti-' + (c.atingido ? 'circle-check' : 'circle-x') + '" style="font-size:13px;color:' + (c.atingido ? 'var(--gold-soft)' : '#E2A33D') + ';vertical-align:-2px;margin-right:6px;"></i>' + c.nome + '</span><span class="tag">' + c.detalhe + '</span></div>';
  }).join('');
  html += '<div class="' + (elegibilidade.elegivel ? 'insight' : 'info-box') + '" style="margin-top:8px;"><p' + (elegibilidade.elegivel ? '' : ' class="txt"') + '>' + (elegibilidade.elegivel ? 'Critérios atendidos, pode promover pra intermediário quando confirmar.' : 'Ainda faltam critérios pra promoção de nível.') + '</p></div>';
  return html;
}

function renderBlocoPeriodizacao(a){
  if(!a.treinoAtual) return '';
  const info = calcularBlocoAtual(a);
  return '<p class="section-label" style="margin-top:22px;">Periodização</p>' +
    '<div class="badge">' + info.bloco + '</div>' +
    '<p class="txt" style="font-size:12px;color:var(--text-faint);margin:6px 0 0;">' + info.descricao + '</p>';
}

function renderTecnicaPendente(a){
  if(!a.tecnicaAprovada) return '';
  const pendentes = Object.keys(a.tecnicaAprovada).filter(function(k){ return a.tecnicaAprovada[k] === 'pendente'; });
  if(pendentes.length === 0) return '';
  let html = '<p class="section-label" style="margin-top:22px;">Técnica aguardando vídeo</p>';
  pendentes.forEach(function(nomeEx){
    html += '<div class="list-item"><span><i class="ti ti-video" style="font-size:13px;color:var(--gold-soft);vertical-align:-2px;margin-right:6px;"></i>' + nomeEx + '</span><span class="acao-pill" onclick="aprovarTecnica(\'' + a.nome.replace(/'/g,"\\'") + '\',\'' + nomeEx.replace(/'/g,"\\'") + '\')">Aprovar</span></div>';
  });
  return html;
}

function renderDirecionamentoTecnico(a){
  const dQuad = a.direcionamentoQuadriceps || 'nenhum';
  const dGluteo = a.direcionamentoGluteo || 'nenhum';
  const sugQuad = sugerirExerciciosQuadriceps(dQuad);
  const sugGluteo = sugerirExerciciosGluteo(dGluteo);

  return '<p class="section-label" style="margin-top:22px;">Direcionamento técnico</p>' +
    '<div class="form-group"><label class="form-label">Precisa desenvolver região distal do quadríceps (perto do joelho)?</label>' +
      '<select class="form-select" id="select-quad-' + a.nome.replace(/[^a-zA-Z0-9]/g,'') + '" onchange="salvarDirecionamento(\'' + a.nome.replace(/'/g,"\\'") + '\', \'quadriceps\', this.value)">' +
        '<option value="nenhum"' + (dQuad === 'nenhum' ? ' selected' : '') + '>Não, manter multiarticulares</option>' +
        '<option value="distal"' + (dQuad === 'distal' ? ' selected' : '') + '>Sim, priorizar região distal</option>' +
      '</select></div>' +
    '<div class="info-box" style="margin-bottom:14px;"><p class="txt">Prioridade: ' + sugQuad.prioridade.join(', ') + (sugQuad.nota ? '<br><span style="color:var(--text-faint);font-size:11px;">' + sugQuad.nota + '</span>' : '') + '</p></div>' +

    '<div class="form-group"><label class="form-label">Região do glúteo a priorizar</label>' +
      '<select class="form-select" id="select-gluteo-' + a.nome.replace(/[^a-zA-Z0-9]/g,'') + '" onchange="salvarDirecionamento(\'' + a.nome.replace(/'/g,"\\'") + '\', \'gluteo\', this.value)">' +
        '<option value="nenhum"' + (dGluteo === 'nenhum' ? ' selected' : '') + '>Sem prioridade específica</option>' +
        '<option value="inferior"' + (dGluteo === 'inferior' ? ' selected' : '') + '>Região inferior (prega glútea)</option>' +
        '<option value="superior"' + (dGluteo === 'superior' ? ' selected' : '') + '>Região superior/lateral (glúteo médio)</option>' +
        '<option value="ambas"' + (dGluteo === 'ambas' ? ' selected' : '') + '>Ambas</option>' +
      '</select></div>' +
    '<div class="info-box"><p class="txt">' + sugGluteo.regiao + '<br>Prioridade: ' + sugGluteo.prioridade.join(', ') + '</p></div>';
}

function salvarDirecionamento(nomeAluna, tipo, valor){
  const a = alunasPersonal.find(function(x){ return x.nome === nomeAluna; });
  if(!a) return;
  if(tipo === 'quadriceps') a.direcionamentoQuadriceps = valor;
  if(tipo === 'gluteo') a.direcionamentoGluteo = valor;
  salvarPerfilAlunaNoSupabase(nomeAluna);
  const i = alunasPersonal.indexOf(a);
  openAlunaDetail(i);
}

const categoriasDesviosPosturais = {
  'Cervical': ['cabeca_anteriorizada'],
  'Ombros/Escápula': ['ombros_protrusos', 'escapula_alada', 'aducao_escapular', 'elevacao_escapular', 'deslizamento_anterior_umero', 'rotacao_inferior_escapular'],
  'Torácica': ['hipercifose', 'escoliose_leve', 'retificacao_toracica'],
  'Lombar': ['hiperlordose', 'retroversao_pelvica', 'anteversao_pelvica', 'sway_back', 'retificacao_lombar'],
  'Quadril': ['assimetria_quadril'],
  'Joelhos': ['geno_valgo', 'geno_varo'],
  'Tornozelos': ['pe_pronado', 'pe_supinado']
};
const categoriasDesvioAbertas = {}; // guarda o estado (aberta/fechada) de cada categoria, sobrevive a recarregar a ficha

function alternarCategoriaDesvioAberta(nomeAluna, categoria){
  categoriasDesvioAbertas[categoria] = !categoriasDesvioAbertas[categoria];
  const id = 'categoria-desvio-' + categoria.replace(/[^a-zA-Z0-9]/g, '');
  const el = document.getElementById(id);
  if(el) el.style.display = categoriasDesvioAbertas[categoria] ? 'flex' : 'none';
}

function renderAvaliacaoPostural(a){
  if(!a.desviosPosturaisConfirmados) a.desviosPosturaisConfirmados = [];

  let conteudoInterno = '<p class="page-sub" style="margin-top:-4px;">Escolhe a região, depois marca os desvios identificados — o sistema já relaciona a mobilidade e o corretivo certos, direto no treino do dia dela.</p>' +
    '<div style="display:flex;flex-wrap:wrap;gap:8px;margin-bottom:10px;">';

  Object.keys(categoriasDesviosPosturais).forEach(function(categoria){
    const idsDaCategoria = categoriasDesviosPosturais[categoria];
    const qtdAtivosNaCategoria = idsDaCategoria.filter(function(id){ return a.desviosPosturaisConfirmados.indexOf(id) !== -1; }).length;
    conteudoInterno += '<span class="chip" style="cursor:pointer;' + (qtdAtivosNaCategoria > 0 ? 'background:var(--gold-soft);color:#1A1409;' : '') + '" onclick="alternarCategoriaDesvioAberta(\'' + a.nome.replace(/'/g,"\\'") + '\',\'' + categoria + '\')">' + categoria + (qtdAtivosNaCategoria > 0 ? ' (' + qtdAtivosNaCategoria + ')' : '') + '</span>';
  });
  conteudoInterno += '</div>';

  Object.keys(categoriasDesviosPosturais).forEach(function(categoria){
    const idDiv = 'categoria-desvio-' + categoria.replace(/[^a-zA-Z0-9]/g, '');
    conteudoInterno += '<div id="' + idDiv + '" style="display:' + (categoriasDesvioAbertas[categoria] ? 'flex' : 'none') + ';flex-direction:column;gap:5px;margin-bottom:12px;">';
    categoriasDesviosPosturais[categoria].forEach(function(idDesvio){
      const d = desviosPosturaisCatalogo.find(function(x){ return x.id === idDesvio; });
      if(!d) return;
      const ativo = a.desviosPosturaisConfirmados.indexOf(d.id) !== -1;
      conteudoInterno += '<div style="background:' + (ativo ? 'var(--success-soft)' : 'var(--card-2)') + ';border:1px solid ' + (ativo ? 'var(--success)' : 'var(--border)') + ';border-radius:10px;padding:8px 10px;cursor:pointer;font-size:12px;display:flex;justify-content:space-between;align-items:center;" onclick="alternarDesvioPostural(\'' + a.nome.replace(/'/g,"\\'") + '\',\'' + d.id + '\')">' +
        '<span>' + d.nome + '</span>' +
        (ativo ? '<i class="ti ti-check" style="color:var(--success);font-size:14px;"></i>' : '') +
      '</div>';
    });
    conteudoInterno += '</div>';
  });

  if(a.desviosPosturaisConfirmados.length > 0){
    const bloco = obterBlocoPostural(a.desviosPosturaisConfirmados);
    conteudoInterno += '<div class="info-box"><p class="lbl">Bloco de mobilidade/correção que vai aparecer no treino dela</p>';
    bloco.forEach(function(ex){ conteudoInterno += '<p class="txt">• ' + ex.nome + ', ' + ex.volume + ' · descanso ' + ex.descanso + '</p>'; });
    conteudoInterno += '</div>';
  }

  const rotulo = a.desviosPosturaisConfirmados.length > 0 ? 'Avaliação postural (' + a.desviosPosturaisConfirmados.length + ' confirmado(s))' : 'Avaliação postural (a partir da anamnese/fotos)';
  return renderSecaoColapsavel(rotulo, conteudoInterno, 'avaliacaopostural-' + a.nome.replace(/[^a-zA-Z0-9]/g,''));
}

function alternarDesvioPostural(nomeAluna, desvioId){
  const a = alunasPersonal.find(function(x){ return x.nome === nomeAluna; });
  if(!a) return;
  if(!a.desviosPosturaisConfirmados) a.desviosPosturaisConfirmados = [];
  const idx = a.desviosPosturaisConfirmados.indexOf(desvioId);
  if(idx === -1){ a.desviosPosturaisConfirmados.push(desvioId); } else { a.desviosPosturaisConfirmados.splice(idx, 1); }
  salvarPerfilAlunaNoSupabase(nomeAluna);
  const i = alunasPersonal.indexOf(a);
  openAlunaDetail(i);
}

function gerarLinhaDoTempo(a){
  const prog = getProgressoAluna(a.nome);
  const eventos = [];

  (prog.feedbackTreino || []).forEach(function(f){
    let texto = f.dia + ': intensidade ' + (f.intensidade != null ? f.intensidade + '/10' : 'não informada');
    let icone = 'mood-smile';
    if(f.desconforto){
      texto += ' · desconforto em ' + f.exercicio + ' (' + f.escalaDesconforto + '/10)';
      icone = 'alert-triangle';
    }
    eventos.push({ semana: f.semana, icone: icone, texto: texto });
  });

  Object.keys(prog.diasConcluidos || {}).forEach(function(semana){
    const dias = prog.diasConcluidos[semana];
    const total = totalDiasDeTreino();
    const completa = dias.length >= total;
    eventos.push({ semana: parseInt(semana, 10), icone: completa ? 'circle-check' : 'circle-x', texto: 'Semana ' + semana + ': ' + dias.length + '/' + total + ' treinos concluídos' + (completa ? '' : ', reengajamento disparado') });
  });

  Object.keys(prog.historico || {}).forEach(function(nomeEx){
    prog.historico[nomeEx].forEach(function(r){
      if(r.sugestao.texto === 'Aumentar carga'){
        eventos.push({ semana: r.semana, icone: 'trending-up', texto: 'Progressão de carga em ' + nomeEx + ' → ' + r.sugestao.valor + 'kg' });
      }
    });
  });

  (a.pesoHistorico || []).forEach(function(p){
    eventos.push({ semana: p.semana, icone: 'scale', texto: 'Peso registrado: ' + p.peso + 'kg' });
  });

  (a.desviosPosturaisConfirmados || []).forEach(function(id){
    const d = desviosPosturaisCatalogo.find(function(x){ return x.id === id; });
    if(d) eventos.push({ semana: 0, icone: 'yoga', texto: 'Desvio postural confirmado: ' + d.nome });
  });

  if(a.patologiaConfirmada && a.patologiaConfirmada !== 'tecnica'){
    const p = patologiasCatalogo.find(function(x){ return x.id === a.patologiaConfirmada; });
    if(p) eventos.push({ semana: 0, icone: 'first-aid-kit', texto: 'Patologia confirmada: ' + p.nome });
  }

  if(a.tecnicaAprovada){
    Object.keys(a.tecnicaAprovada).forEach(function(ex){
      if(a.tecnicaAprovada[ex] === 'aprovado'){
        eventos.push({ semana: 0, icone: 'video', texto: 'Técnica aprovada: ' + ex });
      }
    });
  }

  eventos.sort(function(x, y){ return x.semana - y.semana; });
  return eventos;
}

function renderGraficoPeso(a){
  const historico = a.pesoHistorico || [];
  if(historico.length < 2) return '';
  const pesos = historico.map(function(p){ return p.peso; });
  const min = Math.min.apply(null, pesos) - 1;
  const max = Math.max.apply(null, pesos) + 1;
  const largura = 280, altura = 90, padding = 10;
  const pontos = historico.map(function(p, i){
    const x = padding + (i / (historico.length - 1)) * (largura - padding * 2);
    const y = altura - padding - ((p.peso - min) / (max - min)) * (altura - padding * 2);
    return { x: x, y: y, peso: p.peso, semana: p.semana };
  });
  const linha = pontos.map(function(p, i){ return (i === 0 ? 'M' : 'L') + p.x.toFixed(1) + ',' + p.y.toFixed(1); }).join(' ');
  let svg = '<svg width="100%" viewBox="0 0 ' + largura + ' ' + altura + '" style="display:block;">' +
    '<path d="' + linha + '" fill="none" stroke="#E8C58A" stroke-width="2"/>';
  pontos.forEach(function(p){
    svg += '<circle cx="' + p.x.toFixed(1) + '" cy="' + p.y.toFixed(1) + '" r="3" fill="#F4D9A5"/>' +
      '<text x="' + p.x.toFixed(1) + '" y="' + (p.y - 8).toFixed(1) + '" font-size="9" fill="var(--text-faint)" text-anchor="middle">' + p.peso + '</text>';
  });
  svg += '</svg>';
  return '<p class="lbl" style="margin-top:14px;">Evolução de peso</p><div class="info-box">' + svg + '</div>';
}

function renderLinhaDoTempo(a){
  const eventos = gerarLinhaDoTempo(a);
  let html = '<p class="section-label" style="margin-top:22px;">Linha do tempo</p>';
  html += renderGraficoPeso(a);
  if(eventos.length === 0){
    html += '<div class="info-box"><p class="txt" style="margin-bottom:0;">Ainda sem eventos registrados, eles vão aparecer aqui conforme ela for treinando, registrando peso, e conforme você for confirmando avaliações.</p></div>';
    return html;
  }
  const coresPorIcone = {
    'circle-check': '#E8C58A', 'circle-x': '#E2A33D', 'trending-up': '#E8C58A',
    'scale': '#8AB4D9', 'yoga': '#B893D9', 'first-aid-kit': '#E2A33D',
    'video': '#E8C58A', 'mood-smile': '#E8C58A', 'alert-triangle': '#E2A33D'
  };
  html += '<div style="position:relative;padding-left:20px;margin-top:10px;">' +
    '<div style="position:absolute;left:5px;top:6px;bottom:6px;width:2px;background:var(--border);"></div>';
  eventos.forEach(function(ev){
    const cor = coresPorIcone[ev.icone] || 'var(--gold-soft)';
    html += '<div style="position:relative;padding-bottom:16px;">' +
      '<div style="position:absolute;left:-20px;top:3px;width:10px;height:10px;border-radius:50%;background:' + cor + ';border:2px solid var(--bg);"></div>' +
      '<div style="display:flex;justify-content:space-between;align-items:start;gap:8px;">' +
        '<span style="font-size:13px;color:var(--text);">' + ev.texto + '</span>' +
        (ev.semana > 0 ? '<span class="tag" style="flex-shrink:0;">Semana ' + ev.semana + '</span>' : '') +
      '</div>' +
    '</div>';
  });
  html += '</div>';
  return html;
}

const DURACAO_PLANO_DIAS = 180; // 6 meses, usado só se a aluna ainda não tiver plano real cadastrado

function calcularFaseFunil(diasNoPrograma, duracaoPlano){
  duracaoPlano = duracaoPlano || DURACAO_PLANO_DIAS;
  const diasRestantes = duracaoPlano - diasNoPrograma;
  if(diasNoPrograma <= 40){
    return { fase: 'Boas-vindas', mensagem: 'Funil intensivo de acolhimento (primeiros 40 dias)', dias: diasNoPrograma };
  }
  if(diasRestantes <= -10){
    return { fase: 'Carência expirada', mensagem: 'Passou dos 10 dias de carência, reavaliar renovação', dias: diasNoPrograma };
  }
  if(diasRestantes <= 0){
    return { fase: 'Carência', mensagem: 'Dentro do período de carência (10 dias após o vencimento)', dias: diasNoPrograma };
  }
  if(diasRestantes <= 45){
    return { fase: 'Pré-renovação', mensagem: 'Funil de renovação ativo, ' + diasRestantes + ' dias até vencer', dias: diasNoPrograma };
  }
  return { fase: 'Ritmo reduzido', mensagem: 'Comunicação de manutenção, ritmo reduzido', dias: diasNoPrograma };
}

function gerarSenhaAleatoria(){
  const chars = 'ABCDEFGHJKLMNPQRSTUVWXYZabcdefghijkmnpqrstuvwxyz23456789';
  let senha = '';
  for(let i = 0; i < 8; i++) senha += chars.charAt(Math.floor(Math.random() * chars.length));
  return senha;
}

async function criarLoginParaAluna(nomeAluna){
  const a = alunasPersonal.find(function(x){ return x.nome === nomeAluna; });
  if(!a) return;
  const areaResultado = document.getElementById('acesso-gerado-area');

  if(!a.email){
    areaResultado.innerHTML = '<div class="info-box"><p class="txt" style="color:#E2A33D;">Ela não tem e-mail cadastrado na anamnese, não dá pra criar login sem isso.</p></div>';
    return;
  }
  if(!supabaseClient){
    areaResultado.innerHTML = '<div class="info-box"><p class="txt" style="color:#E2A33D;">Supabase não carregou nesse ambiente agora, tenta de novo.</p></div>';
    return;
  }

  areaResultado.innerHTML = '<p class="txt" style="color:var(--text-faint);">Criando acesso...</p>';

  try {
    const senha = gerarSenhaAleatoria();
    // Cliente temporário e isolado, pra não perder a sua sessão de personal ao criar a conta dela
    const clienteTemp = window.supabase.createClient(SUPABASE_URL, SUPABASE_ANON_KEY, { auth: { persistSession: false } });
    const { data, error } = await clienteTemp.auth.signUp({ email: a.email, password: senha });

    if(error){
      if(error.message.indexOf('already registered') !== -1){
        const { data: alunaRow } = await supabaseClient.from('alunas').select('email,senha_gerada,auth_id').eq('email', a.email).maybeSingle();
        if(alunaRow && alunaRow.senha_gerada){
          areaResultado.innerHTML = '<div class="info-box"><p class="lbl" style="color:var(--success);">✓ Ela já tem acesso criado</p><p class="txt">' + alunaRow.email + '<br>' + alunaRow.senha_gerada + '</p></div>';
        } else {
          areaResultado.innerHTML = '<div class="info-box"><p class="txt" style="color:#E2A33D;">Já existe conta com esse e-mail, mas não achei a senha salva pra mostrar (foi criada de outro jeito, antes desse recurso existir). Ela precisa redefinir a senha pra conseguir entrar.</p></div>';
        }
      } else {
        areaResultado.innerHTML = '<div class="info-box"><p class="txt" style="color:#E2A33D;">' + error.message + '</p></div>';
      }
      return;
    }

    // Cria o perfil dela, usando a sua sessão de personal (tem permissão via RLS)
    await supabaseClient.from('perfis').insert({ id: data.user.id, tipo: 'aluna', nome: a.nome });
    // Liga o login novo aos dados dela (se já existirem da anamnese) ou cria do zero
    await supabaseClient.from('alunas').upsert({
      email: a.email, auth_id: data.user.id, nome: a.nome, telefone: a.telefone || '',
      nivel: a.nivel || 'Iniciante', freq: a.freq || '3x por semana', piramide: a.piramide || '',
      objetivo: a.objetivo || '', restricoes: a.restricoes || '', academia: a.academia || '',
      tempo_disponivel: a.tempoDisponivel || 60,
      senha_gerada: senha
    }, { onConflict: 'email' });

    // Atualiza a ficha local na hora, sem precisar recarregar a página
    a.authId = data.user.id;
    a.senhaGerada = senha;

    const linkApp = LINK_DO_APP;

    areaResultado.innerHTML = '<div class="info-box">' +
      '<p class="txt" style="font-weight:600;">Acesso criado</p>' +
      '<p class="txt">E-mail: ' + a.email + '<br>Senha: <b>' + senha + '</b></p>' +
      '<p class="txt" style="font-size:11px;color:var(--text-faint);">Link do app: ' + linkApp + '</p>' +
      '</div>' +
      '<button class="btn-gold" style="width:auto;padding:8px 14px;margin:0 8px 0 0;font-size:12px;background:#25D366;color:#fff;border:none;" onclick="enviarCredenciaisPorWhatsApp(\'' + a.nome.replace(/'/g,"\\'") + '\')"><i class="ti ti-brand-whatsapp" style="vertical-align:-2px;margin-right:4px;"></i>Mandar por WhatsApp</button>' +
    '<button class="btn-gold" style="width:auto;padding:8px 14px;margin:0;font-size:12px;background:var(--card-2);color:var(--gold-soft);border:1px solid var(--border);" onclick="navigator.clipboard.writeText(\'E-mail: ' + a.email + ' - Senha: ' + senha + ' - Link: ' + linkApp + '\')">Copiar dados</button>';
  } catch(erroDeRede){
    console.warn('Sem conexão com o Supabase agora:', erroDeRede);
    areaResultado.innerHTML = '<div class="info-box"><p class="txt" style="color:#E2A33D;">Sem conexão com o banco de dados agora (isso só deveria acontecer aqui no ambiente de teste, não depois de publicado). Tenta de novo em alguns segundos.</p></div>';
  }
}

async function enviarCredenciaisPorWhatsApp(nomeAluna){
  const a = alunasPersonal.find(function(x){ return x.nome === nomeAluna; });
  const area = document.getElementById('acesso-gerado-area');
  if(!a || !a.senhaGerada){ if(area) area.innerHTML = '<div class="info-box"><p class="txt" style="color:#C9784A;">Ainda não tem acesso gerado pra essa aluna.</p></div>'; return; }
  if(!a.telefone){ if(area) area.innerHTML = '<div class="info-box"><p class="txt" style="color:#C9784A;">Essa aluna não tem telefone cadastrado, não dá pra mandar por WhatsApp.</p></div>'; return; }

  if(area) area.innerHTML = '<p class="txt" style="color:var(--text-faint);">Enviando...</p>';
  const linkApp = LINK_DO_APP;
  const mensagem = 'Oi ' + a.nome.split(' ')[0] + '! Seu acesso ao MUSA+ já está pronto.\nE-mail: ' + a.email + '\nSenha: ' + a.senhaGerada + '\nEntre aqui: ' + linkApp;
  const saida = await enviarWhatsApp(a.telefone, mensagem);

  if(!area) return;
  if(saida.sucesso){
    area.innerHTML = '<div class="info-box" style="border-color:var(--success);"><p class="txt" style="color:var(--success);">✓ Enviado pro WhatsApp dela.</p></div>';
  } else {
    area.innerHTML = '<div class="info-box" style="border-color:#C9784A;"><p class="txt" style="color:#C9784A;">Não consegui enviar: ' + (saida.motivo || 'motivo não informado') + '</p></div>';
  }
}

function renderPlanoFechadoConteudo(a){
  const planos = { '30': '30 dias', '90': '90 dias (trimestral)', '180': '180 dias (semestral)', '365': '365 dias (anual)' };
  const duracaoAtual = a.duracaoPlanoDias || '';
  return '<div class="form-group"><label class="form-label">Quando ela fechou o plano?</label>' +
      '<input class="form-input" type="date" id="plano-data-' + a.nome.replace(/[^a-zA-Z0-9]/g,'') + '" value="' + (a.dataFechouPlano || '') + '" onchange="salvarPlanoFechado(\'' + a.nome.replace(/'/g,"\\'") + '\',\'data\',this.value)"></div>' +
    '<div class="form-group"><label class="form-label">Qual plano / período?</label>' +
      '<select class="form-select" id="plano-duracao-' + a.nome.replace(/[^a-zA-Z0-9]/g,'') + '" onchange="salvarPlanoFechado(\'' + a.nome.replace(/'/g,"\\'") + '\',\'duracao\',this.value)">' +
        '<option value="">Selecione...</option>' +
        Object.keys(planos).map(function(k){ return '<option value="' + k + '"' + (String(duracaoAtual) === k ? ' selected' : '') + '>' + planos[k] + '</option>'; }).join('') +
      '</select></div>' +
    '<div class="form-group"><label class="form-label">Valor do plano (R$)</label>' +
      '<input class="form-input" type="number" step="0.01" min="0" placeholder="Ex: 350.00" id="plano-valor-' + a.nome.replace(/[^a-zA-Z0-9]/g,'') + '" value="' + (a.valorPlano != null ? a.valorPlano : '') + '" onchange="salvarPlanoFechado(\'' + a.nome.replace(/'/g,"\\'") + '\',\'valor\',this.value)"></div>' +
    '<div class="form-group"><label class="form-label">Pagamento</label>' +
      '<select class="form-select" id="plano-pagamento-' + a.nome.replace(/[^a-zA-Z0-9]/g,'') + '" onchange="salvarPlanoFechado(\'' + a.nome.replace(/'/g,"\\'") + '\',\'pagamento\',this.value)">' +
        '<option value="pago"' + ((a.statusPagamento || 'pago') === 'pago' ? ' selected' : '') + '>Pago</option>' +
        '<option value="pendente"' + (a.statusPagamento === 'pendente' ? ' selected' : '') + '>Pendente</option>' +
      '</select></div>';
}

function renderRodaDaVidaNaFicha(nomeAluna){
  const registro = getUltimaRodaDaVidaPreenchida(nomeAluna);
  if(!registro || Object.keys(registro.areas).length === 0){
    return '<div class="info-box"><p class="txt" style="color:var(--text-faint);">A aluna ainda não preencheu a Roda da Vida.</p></div>';
  }
  const nomeDoMes = new Date(registro.mes + '-01T00:00:00').toLocaleDateString('pt-BR', { month: 'long', year: 'numeric' });
  const resumo = calcularPontoForteEAreaFraca(registro.areas);
  let html = '<div class="info-box" style="margin-bottom:10px;">' +
    '<p class="lbl" style="margin-bottom:8px;">Última avaliação: ' + nomeDoMes.charAt(0).toUpperCase() + nomeDoMes.slice(1) + '</p>' +
    gerarSvgRadar(registro.areas, 220) +
  '</div>';
  if(resumo){
    html += '<div class="row" style="gap:8px;">' +
      '<div class="info-box" style="flex:1;background:var(--success-soft);border-color:var(--success);"><p class="lbl" style="font-size:11px;">Ponto forte</p><p class="txt" style="font-size:12px;">' + resumo.forte.label + ' (' + resumo.forte.valor + '/5)</p></div>' +
      '<div class="info-box" style="flex:1;"><p class="lbl" style="font-size:11px;">Área pra focar</p><p class="txt" style="font-size:12px;">' + resumo.fraca.label + ' (' + resumo.fraca.valor + '/5)</p></div>' +
    '</div>';
  }
  return html;
}

function salvarPlanoFechado(nomeAluna, campo, valor){
  const a = alunasPersonal.find(function(x){ return x.nome === nomeAluna; });
  if(!a) return;
  if(campo === 'data') a.dataFechouPlano = valor;
  if(campo === 'duracao') a.duracaoPlanoDias = parseInt(valor, 10) || null;
  if(campo === 'valor') a.valorPlano = valor === '' ? null : parseFloat(valor);
  if(campo === 'pagamento') a.statusPagamento = valor;
  salvarPerfilAlunaNoSupabase(nomeAluna);
  const i = alunasPersonal.indexOf(a);
  openAlunaDetail(i);
  renderAlunas();
}

// ===== STATUS FINANCEIRO (vencimento + pagamento combinados) =====
function statusFinanceiroAluna(a){
  if(statusDoPlano(a) === 'vencidas') return { label: 'Atrasado', cor: '#C9784A', corSuave: 'rgba(201,120,74,0.15)' };
  if(a.statusPagamento === 'pendente') return { label: 'Pendente', cor: '#E2A33D', corSuave: 'rgba(226,163,61,0.15)' };
  return { label: 'Ativo', cor: 'var(--success)', corSuave: 'var(--success-soft)' };
}

function renderFunilEngajamento(a){
  let diasNoPrograma, duracaoPlano;
  if(a.dataFechouPlano && a.duracaoPlanoDias){
    const inicio = new Date(a.dataFechouPlano + 'T00:00:00');
    const hoje = new Date();
    diasNoPrograma = Math.max(0, Math.round((hoje - inicio) / (1000*60*60*24)));
    duracaoPlano = a.duracaoPlanoDias;
  } else {
    if(a.diasNoProgramaFunil == null) a.diasNoProgramaFunil = 1;
    diasNoPrograma = a.diasNoProgramaFunil;
    duracaoPlano = DURACAO_PLANO_DIAS;
  }
  const info = calcularFaseFunil(diasNoPrograma, duracaoPlano);
  const avisoSemPlano = (!a.dataFechouPlano || !a.duracaoPlanoDias) ? '<p class="txt" style="color:var(--text-faint);font-size:11px;">Sem plano cadastrado ainda, usando simulação genérica de 180 dias. Preencha "Plano fechado" acima pra ficar preciso.</p>' : '';
  return '<p class="section-label" style="margin-top:22px;">Funil de engajamento e renovação</p>' +
    '<div class="badge">' + info.fase + '</div>' +
    '<p class="txt" style="font-size:12px;color:var(--text-faint);margin:6px 0;">' + info.mensagem + ' (dia ' + info.dias + ' de ' + duracaoPlano + ')</p>' +
    avisoSemPlano +
    (!a.dataFechouPlano ? '<button class="btn-gold" style="background:var(--card-2);color:var(--gold-soft);border:1px solid var(--border);" onclick="avancarDiasFunil(\'' + a.nome.replace(/'/g,"\\'") + '\', 10)">Simular +10 dias</button>' : '');
}

function renderDuvidasSinalizadas(a){
  if(!a.duvidasSinalizadas || a.duvidasSinalizadas.length === 0) return '';
  const pendentes = a.duvidasSinalizadas.filter(function(d){ return !d.resolvida; });
  if(pendentes.length === 0) return '';
  let html = '<p class="section-label" style="margin-top:22px;">Dúvidas que a Sol sinalizou pra você</p>';
  pendentes.forEach(function(d){
    const idxReal = a.duvidasSinalizadas.indexOf(d);
    const mensagemWpp = encodeURIComponent('Oi ' + a.nome.split(' ')[0] + '! Vi sua pergunta: "' + d.pergunta + '". Deixa eu te explicar melhor:');
    const linkWpp = 'https://wa.me/55' + (a.telefone || '').replace(/\D/g, '') + '?text=' + mensagemWpp;
    html += '<div class="info-box">' +
      '<p class="lbl">Pergunta da aluna</p>' +
      '<p class="txt">"' + d.pergunta + '"</p>' +
      '<p class="lbl" style="margin-top:8px;">O que a Sol respondeu</p>' +
      '<p class="txt">' + d.respostaSol + '</p>' +
      '<div style="display:flex;gap:8px;margin-top:10px;">' +
        '<a class="btn-gold" style="width:auto;padding:8px 14px;margin:0;font-size:12px;background:#25D366;color:#fff;text-decoration:none;" href="' + linkWpp + '" target="_blank" rel="noopener"><i class="ti ti-brand-whatsapp" style="vertical-align:-2px;margin-right:4px;"></i>Avisar no WhatsApp</a>' +
        '<button class="btn-gold" style="width:auto;padding:8px 14px;margin:0;font-size:12px;background:var(--card-2);color:var(--gold-soft);border:1px solid var(--border);" onclick="marcarDuvidaResolvida(\'' + a.nome.replace(/'/g,"\\'") + '\',' + idxReal + ')">Marcar como resolvida</button>' +
      '</div>' +
    '</div>';
  });
  return html;
}

function marcarDuvidaResolvida(nomeAluna, idx){
  const a = alunasPersonal.find(function(x){ return x.nome === nomeAluna; });
  if(!a || !a.duvidasSinalizadas || !a.duvidasSinalizadas[idx]) return;
  a.duvidasSinalizadas[idx].resolvida = true;
  salvarPerfilAlunaNoSupabase(nomeAluna);
  const i = alunasPersonal.indexOf(a);
  openAlunaDetail(i);
}

function avancarDiasFunil(nomeAluna, dias){
  const a = alunasPersonal.find(function(x){ return x.nome === nomeAluna; });
  if(!a) return;
  a.diasNoProgramaFunil = (a.diasNoProgramaFunil || 1) + dias;
  salvarPerfilAlunaNoSupabase(nomeAluna);
  const i = alunasPersonal.indexOf(a);
  openAlunaDetail(i);
}

function renderElegibilidadeFase(a){
  const elegibilidade = calcularElegibilidadeFase(a.nome);
  if(!elegibilidade) return '';
  let html = '<p class="section-label" style="margin-top:22px;">Avaliação de mudança de fase</p>';
  html += elegibilidade.criterios.map(function(c){
    return '<div class="list-item"><span><i class="ti ti-' + (c.atingido ? 'circle-check' : 'circle-x') + '" style="font-size:13px;color:' + (c.atingido ? 'var(--gold-soft)' : '#E2A33D') + ';vertical-align:-2px;margin-right:6px;"></i>' + c.nome + '</span><span class="tag">' + c.detalhe + '</span></div>';
  }).join('');
  html += '<div class="' + (elegibilidade.elegivel ? 'insight' : 'info-box') + '" style="margin-top:8px;"><p' + (elegibilidade.elegivel ? '' : ' class="txt"') + '>' + (elegibilidade.elegivel ? 'Critérios atendidos, pode avançar de fase quando confirmar.' : 'Ainda faltam critérios, o sistema não sugere avanço de fase até todos serem atingidos.') + '</p></div>';
  return html;
}

const ferramentasPersonal = [
  { titulo: 'Alunas', icone: 'ti-users', view: 'alunas' },
  { titulo: 'Alunas antigas (2024-)', icone: 'ti-history', view: 'alunas', acaoEspecial: 'abrirAlunasAntigas' },
  { titulo: 'Banco de exercícios', icone: 'ti-video', view: 'exercicios' },
  { titulo: 'Biblioteca de conteúdo', icone: 'ti-library', view: 'conteudo' },
  { titulo: 'Biblioteca de treinos', icone: 'ti-clipboard-list', view: 'treinos' },
  { titulo: 'Biblioteca de mobilidade', icone: 'ti-stretching', view: 'mobilidade' },
  { titulo: 'Grupo de desafio', icone: 'ti-flag', view: 'desafios' },
  { titulo: 'Banco de patologias', icone: 'ti-first-aid-kit', view: 'patologias' },
  { titulo: 'Banco de desvios posturais', icone: 'ti-yoga', view: 'desvios' },
  { titulo: 'Banco de testes de corrida', icone: 'ti-run', view: 'corrida' }
];

function renderFerramentasPersonal(){
  const grid = document.getElementById('grid-ferramentas-personal');
  if(!grid || grid.dataset.rendered) return;
  grid.dataset.rendered = 'true';
  grid.style.gridTemplateColumns = '1fr 1fr 1fr';
  ferramentasPersonal.forEach(function(f){
    const el = document.createElement('div');
    el.className = 'ferramenta-compacta';
    el.innerHTML = '<i class="ti ' + f.icone + '"></i><p>' + f.titulo + '</p>';
    el.onclick = function(){ f.acaoEspecial ? window[f.acaoEspecial]() : showPersonalView(f.view); };
    grid.appendChild(el);
  });
  renderSidebarPersonal();
}

function renderSidebarPersonal(){
  const sidebar = document.getElementById('sidebar-personal');
  if(!sidebar || sidebar.dataset.rendered) return;
  sidebar.dataset.rendered = 'true';

  let html = '<div class="side-logo">DNA MUSA</div>';
  html += '<div class="side-item" data-side-view="dashboard" onclick="showPersonalView(\'dashboard\')"><i class="ti ti-layout-dashboard"></i>Dashboard</div>';
  html += '<div class="side-item" data-side-view="alunas" onclick="showPersonalView(\'alunas\')"><i class="ti ti-users"></i>Alunas</div>';
  html += '<div class="side-grupo-label">Ferramentas</div>';
  ferramentasPersonal.forEach(function(f){
    html += '<div class="side-item" data-side-view="' + f.view + '" onclick="' + (f.acaoEspecial ? f.acaoEspecial + '()' : "showPersonalView('" + f.view + "')") + '"><i class="ti ' + f.icone + '"></i>' + f.titulo + '</div>';
  });
  html += '<div class="side-item side-sair" onclick="goBack()"><i class="ti ti-logout"></i>Sair</div>';

  sidebar.innerHTML = html;
}

function atualizarSidebarAtiva(which){
  const sidebar = document.getElementById('sidebar-personal');
  if(!sidebar) return;
  sidebar.querySelectorAll('.side-item').forEach(function(el){
    el.classList.toggle('ativo', el.getAttribute('data-side-view') === which);
  });
}

async function sincronizarListaAlunasDoSupabase(){
  if(!supabaseClient) return { novas: 0, erro: 'sem conexão' };
  try {
    const { data: linhas, error } = await supabaseClient.from('alunas').select('*');
    if(error) return { novas: 0, erro: error.message };
    if(!linhas || linhas.length === 0) return { novas: 0, erro: 'busca retornou vazio (pode ser bloqueio de permissão do banco, não deveria estar vazio)' };

    let novas = 0;
    let atualizadas = 0;
    linhas.forEach(function(row){
      const jaExiste = alunasPersonal.find(function(a){ return a.email && row.email && a.email.toLowerCase() === row.email.toLowerCase(); });
      if(jaExiste){
        // Já existe localmente: traz de volta os campos que podem ter sido editados e salvos em outro dispositivo/sessão
        let mudou = false;
        ['nivel','freq','telefone','objetivo','restricoes','academia'].forEach(function(campo){
          const valorBanco = row[campo];
          if(valorBanco != null && valorBanco !== '' && jaExiste[campo] !== valorBanco){
            jaExiste[campo] = valorBanco;
            mudou = true;
          }
        });
        if(row.dados_extras && typeof row.dados_extras === 'object'){
          Object.keys(row.dados_extras).forEach(function(campo){
            const valorBanco = row.dados_extras[campo];
            if(valorBanco != null && JSON.stringify(jaExiste[campo]) !== JSON.stringify(valorBanco)){
              jaExiste[campo] = valorBanco;
              mudou = true;
            }
          });
        }
        if(row.auth_id && jaExiste.authId !== row.auth_id){ jaExiste.authId = row.auth_id; mudou = true; }
        if(row.senha_gerada && jaExiste.senhaGerada !== row.senha_gerada){ jaExiste.senhaGerada = row.senha_gerada; mudou = true; }
        if(mudou) atualizadas++;
        return;
      }

      alunasPersonal.push({
        nome: row.nome,
        email: row.email,
        telefone: row.telefone || '',
        nivel: row.nivel || 'Iniciante',
        freq: row.freq || '3x por semana',
        piramide: row.piramide || '',
        objetivo: row.objetivo || '',
        restricoes: row.restricoes || 'Nenhuma relatada',
        academia: row.academia || '',
        idade: row.idade || null,
        dataAnamnese: row.data_anamnese || new Date().toISOString().slice(0,10),
        status: 'ok',
        statusLabel: 'Ativa recente',
        recemChegadaDaAnamnese: true,
        authId: row.auth_id || null,
        senhaGerada: row.senha_gerada || null
      });
      novas++;
    });

    if(novas > 0 || atualizadas > 0) renderAlunas();
    return { novas: novas, atualizadas: atualizadas, erro: null };
  } catch(erroDeRede){
    return { novas: 0, erro: 'sem conexão com o Supabase agora' };
  }
}

function showPersonalView(which){
  renderFerramentasPersonal();
  atualizarSidebarAtiva(which);
  mostrarSoAlunasAntigas = false; // reseta por padrão sempre — quem quer o modo antigas, define depois de chamar essa função
  ['dashboard','alunas','aluna','resumo-aluna','exercicios','conteudo','treinos','desafios','mobilidade','patologias','desvios','corrida','funil'].forEach(function(v){
    document.getElementById('personal-' + v).style.display = (v === which) ? 'block' : 'none';
  });
  if(which === 'dashboard'){ renderCentralDeAvisos(); renderMetricasNegocio(); renderRelatoriosTendencias(); }
  if(which === 'alunas'){
    renderAlunas();
    const areaDiagnostico = document.createElement('p');
    areaDiagnostico.id = 'diagnostico-sync-alunas';
    areaDiagnostico.style.cssText = 'font-size:11px;color:var(--text-faint);text-align:center;margin:-6px 0 10px;';
    areaDiagnostico.textContent = 'Verificando anamneses novas...';
    document.getElementById('alunas-count-label').insertAdjacentElement('afterend', areaDiagnostico);

    sincronizarListaAlunasDoSupabase().then(function(resultado){
      const el = document.getElementById('diagnostico-sync-alunas');
      if(!el) return;
      if(resultado.erro){
        el.style.color = '#E2A33D';
        el.textContent = 'Não consegui verificar agora: ' + resultado.erro;
      } else if(resultado.novas > 0 || resultado.atualizadas > 0){
        el.style.color = 'var(--success)';
        const partes = [];
        if(resultado.novas > 0) partes.push(resultado.novas + ' nova(s) da anamnese');
        if(resultado.atualizadas > 0) partes.push(resultado.atualizadas + ' atualizada(s)');
        el.textContent = partes.join(' · ') + '.';
        setTimeout(function(){ el.remove(); }, 8000);
      } else {
        el.style.color = 'var(--text-faint)';
        el.textContent = 'Verificado, nenhuma novidade da anamnese no momento.';
        setTimeout(function(){ el.remove(); }, 5000);
      }
    });
  }
  if(which === 'treinos'){ renderTemplates(); populateExercicioSelect(); }
  if(which === 'desafios'){ renderDesafios(); populateTreinoSelect(); }
  if(which === 'mobilidade'){ renderArticulacaoChips(); renderAquecimentoBanco(); document.getElementById('mob-articulacoes-view').style.display = 'block'; document.getElementById('mob-lista-view').style.display = 'none'; }
  if(which === 'patologias'){ renderPatologiaChips(); renderPatologiaList(); }
  if(which === 'desvios'){ renderDesviosBanco(); }
  if(which === 'corrida'){ renderCorridaBanco(); }
  if(which === 'exercicios'){
    document.getElementById('ex-video-view').style.display = 'none';
    document.getElementById('ex-lista-view').style.display = 'block';
    mostrandoFormAdicionar = false;
    renderExerciciosChips();
    carregarCatalogoPersonal();
    renderExerciciosLista();
    populateGrupoSelect();
  }
}

const palavrasNaoSaoExercicio = ['ERRO', 'CORREÇÃO', 'CORREÇÕES', 'CORRECAO', 'CORRECOES', 'AJUSTE', 'FEEDBACK', 'ENTENDA', 'DICA', 'INTRODUÇÃO', 'INTRODUCAO', 'SUPORTE'];
const exerciciosBanco = [
  {nome:'REMADA UNILATERAL COM HALTERES', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=kS0KNWUgU7I'},
  {nome:'REMADA ABERTA COM HALTERES SUP', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=giQfgYKYrGc'},
  {nome:'STIFF UNILATERAL COM HALTERES', grupo:'Isquiotibiais', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=GyDrpYEPWQw'},
  {nome:'STIFF UNILATERAL SEM APOIO', grupo:'Isquiotibiais', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=E6QF1VWS1qY'},
  {nome:'DESENVOLVIMENTO FRONTAL COM MOCHILA', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=RcHEOOACjHk'},
  {nome:'FLEXÃO DE COTOVELO MOCHILA', grupo:'Peito', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=X42BMTwuPMA'},
  {nome:'CADEIRA FLEXORA YES', grupo:'Isquiotibiais', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=ocV8ovygjkM'},
  {nome:'CADEIRA ABDUTORA INCLINADO', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=0ICPVbRiqNM'},
  {nome:'AGACHAMENTO JACK YES', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=jGooGCaCDI8'},
  {nome:'TRICEPS COM BARRA POLIA', grupo:'Tríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=CUCYMqymsZ4'},
  {nome:'TRICEPS POLIA ALTA', grupo:'Tríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=jHd_qtTZINc'},
  {nome:'TRÍCEPS COICE POLIA', grupo:'Tríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=2juXdMcTg28'},
  {nome:'CRUCIFIXO INVERSO COM APOIO', grupo:'Peito', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=5WCO_sbLEAk'},
  {nome:'CRUCIFIXO INVERSO UNILATERAL NA POLIA', grupo:'Peito', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=m85a-zp454s'},
  {nome:'REMADA ABERTA UNILATERAL NA POLIA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=M-sat6gp-HI'},
  {nome:'PULLDOWN COM BARRA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=opdOQXj2Qe8'},
  {nome:'TRICEPS COM BARRA POLIA', grupo:'Tríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=Byff4QmSEc8'},
  {nome:'TRICEPS CORDA POLIA', grupo:'Tríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=wOS8UdARevY'},
  {nome:'PULLDOWN COM CORDA POLIA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=GMonbLwBHqQ'},
  {nome:'FACEPULL COM', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=j_gbLC6eHXQ'},
  {nome:'CADEIRA ABDUTORA INCLINADO - YOUTUBE', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=NQRcrs2NJ5E'},
  {nome:'CADEIRA FLEXORA YES - YOUTUBE', grupo:'Isquiotibiais', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=HxgLLgslej4'},
  {nome:'CRUCIFIXO INVERSO COM APOIO - YOUTUBE', grupo:'Peito', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=IbhLDWWnGxc'},
  {nome:'CRUCIFIXO COM HALTERES - YOUTUBE', grupo:'Peito', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=eHUtTovc3Zg'},
  {nome:'DOUBLE BICEPS - YOUTUBE', grupo:'Bíceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=Sz_DKHXqhBA'},
  {nome:'EXTENSÃO DE QUADRIL POLIA - YOUTUBE', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=uZrno5HI6Cc'},
  {nome:'EXTENSÃO DE QUADRIL POLIA - YOUTUBE', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=uZrno5HI6Cc'},
  {nome:'MANGUITO DEITADO', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=nDUGYVKYGgI&sttick=0'},
  {nome:'PULLDOWN COM BARRA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=OaDynv2jISg&sttick=0'},
  {nome:'REMADA ABERTA COM BARRA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=u51yEma4v3M'},
  {nome:'REMADA ABERTA POLIA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=-AhBdO6ngdE&sttick=0'},
  {nome:'REMADA FECHADA UNILATERAL NA POLIA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=6_sUwRdMZNE&sttick=0'},
  {nome:'PUXADA ABERTA FRONTAL - YOUTUBE', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=QV4WboLAVj0'},
  {nome:'PUSH UP PLUS - VARIAÇÃO - YOUTUBE', grupo:'Peito', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=pdVvEmF97Rw'},
  {nome:'PUSH UP COM BARRA - YOUTUBE', grupo:'Peito', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=wlxPLG--5oo'},
  {nome:'PUSH UP PLUS EM 4 APOIOS', grupo:'Peito', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=87IHSrAHdq8'},
  {nome:'PUSH UP COM BARRA', grupo:'Peito', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=wlxPLG--5oo'},
  {nome:'CRUCIFIXO NA MÁQUINA -FLY', grupo:'Peito', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=L4YtdisHldk'},
  {nome:'ROSCA SCOTT NA MÁQUINA', grupo:'Bíceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=cgciNALjjT4&sttick=0'},
  {nome:'CADEIRA ABDUTORA EM PÉ 45º', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=1Kye0sDSAw8'},
  {nome:'EXTENSÃO DE QUADRIL NA POLIA - TRONCO INCLINADO', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=dvsgo4Gj-hA'},
  {nome:'EXTENSÃO DE QUADRIL NA POLIA - TRONCO INCLINADO', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=dvsgo4Gj-hA'},
  {nome:'EXTENSÃO DE QUADRIL COM CANELEIRA NO BANCO', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=5BuJFrdn_AY'},
  {nome:'PULLOVER COM HALTERES', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=LvrEvExCzV8'},
  {nome:'REMADA ABERTA POLIA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=-AhBdO6ngdE'},
  {nome:'ROSCA CONCENTRADA', grupo:'Bíceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=ROGbtTlxs0Q'},
  {nome:'ROSCA DIRETA 21', grupo:'Bíceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=l-MktIlWRZg'},
  {nome:'STIFF NA POLIA', grupo:'Isquiotibiais', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=lBlWY-illWg'},
  {nome:'TRÍCEPS CORDA POLIA', grupo:'Tríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=--sJN_gWRUc'},
  {nome:'TRICEPS POLIA ALTA', grupo:'Tríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=abdcKPAuyxM'},
  {nome:'CADEIRA ABDUTORA - VARIAÇÃO TRONCO INCLINADO A FRENTE', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=aKzeShloHJs'},
  {nome:'EXTENSÃO DE QUADRIL COM CANELEIRA NO BANCO', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=5BuJFrdn_AY'},
  {nome:'EXTENSÃO DE QUADRIL NA MÁQUINA - VARIAÇÃO PARA MAIOR AMPLITUDE', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=IWpwUAgwnpI'},
  {nome:'ABDUÇÃO DE QUADRIL COM ROTAÇÃO EXTERNA NA POLIA', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=lhw9UXBJ35Q'},
  {nome:'ABDUÇÃO DE QUADRIL COM CANELEIRA', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=fE5qN5-6VUI'},
  {nome:'4 APOIOS COM CANELEIRA - ALTO', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=aOZ5aeoTyX0'},
  {nome:'EXTENSÃO DE QUADRIL CURTINHAS NO ALTO', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=QXrJE4FF-TM'},
  {nome:'PUXADA ABERTA ARTICULADA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=YCK83Z7iQX8'},
  {nome:'SUPINO INCLINADO COM BARRA', grupo:'Peito', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=Py7wmd4Mw1Y'},
  {nome:'LEG PRESS 45º', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=qx2g8W6qoaQ'},
  {nome:'LEG PRESS 45º', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=qx2g8W6qoaQ'},
  {nome:'TRÍCEPS TESTA COM BARRA', grupo:'Tríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=gANUNaWe8WE'},
  {nome:'REMADA FECHADA ARTICULAR - YOUTUBE', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=q-jGW3oHTJg'},
  {nome:'TRÍCEPS FRANCÊS BILATERAL - YOUTUBE', grupo:'Tríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=zogtbslROLk'},
  {nome:'STEP UP', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=NOvtICHyccY'},
  {nome:'CADEIRA EXTENSORA - TRONCO INCLINADO', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=CpQevcceBhg'},
  {nome:'POLICHINELO', grupo:'Aeróbico', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=rJbrYLTNymc'},
  {nome:'AFUNDO COM HALTERES', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=eyhLRHuEjYA'},
  {nome:'TERRA SUMÔ COM BARRA', grupo:'Isquiotibiais', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=xX3YS6MpyTQ'},
  {nome:'LEVANTAMENTO TERRA - YOUTUBE', grupo:'Isquiotibiais', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=w2W-WtAQj2I'},
  {nome:'AGACHAMENTO SUMÔ COM HALTERES - YOUTUBE', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=6zoYEflTEYM'},
  {nome:'RETRAÇÃO DE ESCÁPULAS - YOUTUBE', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=T1MvJi_mCAg'},
  {nome:'VAI E VEM DE FRENTE E COSTAS', grupo:'Aeróbico', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/eQzb92p_lZI'},
  {nome:'PULDOWN COM BARRA NA POLIA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/shlGmlzAUGE'},
  {nome:'RETRAÇÃO DE ESCÁPULAS', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/d15LLZ_Uxvo'},
  {nome:'ROSCA DIRETA ALTERNADA COM ISOMETRIA', grupo:'Bíceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/XYOtweuFkzA'},
  {nome:'ROSCA DIRETA ALTERNADA COM HALTERES', grupo:'Bíceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/Tzz8CUv-LNk'},
  {nome:'EXTENSÃO HORIZONTAL UNILATERAL NA POLIA', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/qCQOT8Py038'},
  {nome:'TRÍCEPS COICE NA POLIA', grupo:'Tríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/uFaZr3cSM7A'},
  {nome:'TRÍCEPS UNILATERAL NA POLIA', grupo:'Tríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/udH-RZ2Wb2Y'},
  {nome:'TRÍCEPS COICE UNILATERAL NA POLIA', grupo:'Tríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/cQ7TeC_rdlg'},
  {nome:'MANGUITO ROTAÇÃO EXTERNA', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/MizDY8p0gDY'},
  {nome:'MANGUITO ROTAÇÃO EXTERNA', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/MizDY8p0gDY'},
  {nome:'DESENVOLVIMENTO FRONTAL COM BARRA', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/n0sRwlEbFRY'},
  {nome:'DESENVOLVIMENTO FRONTAL COM BARRA', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/n0sRwlEbFRY'},
  {nome:'ELEVAÇÃO LATERAL INCLINADA NA ROLDANA', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/IVqSmYZ2awM'},
  {nome:'ELEVAÇÃO LATERAL DECLINADA NA POLIA', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/RKZcUyuDXB4'},
  {nome:'ELEVAÇÃO LATERAL NA POLIA', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/w9ebX7LO9SI'},
  {nome:'ELEVAÇÃO LATERAL NA POLIA', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/w9ebX7LO9SI'},
  {nome:'LEG HORIZONTAL', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/0KLrU6JXOUE'},
  {nome:'PANTURRILHA SENTADO NA MÁQUINA', grupo:'Panturrilha', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/t8vU7HK_wcU'},
  {nome:'REMADA FECHADA COM APOIO', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/6lAxaZwpfDg'},
  {nome:'REMADA ABERTA COM APOIO', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/lmr6pVTTJT0'},
  {nome:'AFUNDO NO SQUAT', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/4NuNYnr1dbs'},
  {nome:'AGACHAMENTO FRONTAL NO SQUAT ( ESTÍMULO GLÚTEOS )', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/-WxH7ND9NEg'},
  {nome:'AGACHAMENTO FRONTAL NO SQUAT ( ESTÍMULO GLÚTEOS )', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/-WxH7ND9NEg'},
  {nome:'STIFF NA MÁQUINA', grupo:'Isquiotibiais', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/iHJxAoJYGBE'},
  {nome:'AGACHAMENTO SUMÔ NA MÁQUINA', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/vMJLDXo8MDw'},
  {nome:'TERRA SUMÔ NA MÁQUINA', grupo:'Isquiotibiais', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/nq14XPTvfHs'},
  {nome:'SUPINO RETO NA MÁQUINA', grupo:'Peito', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/ILPmqVVNDfs'},
  {nome:'SUPINO INCLINADO NA MÁQUINA', grupo:'Peito', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/po7Hmy5Lths'},
  {nome:'SUPINO DECLINADO NA MÁQUINA', grupo:'Peito', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/ULZctWY2bas'},
  {nome:'REMADA FECHADA PRONADA NA MÁQUINA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/aYzUsR5mCY0'},
  {nome:'CADEIRA FLEXORA UNILATERAL', grupo:'Isquiotibiais', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/2YEghbjQQwU'},
  {nome:'AGACHAMENTO PENDULAR', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/zxhM2HGb4tQ'},
  {nome:'REMADA CURVADA ABERTA NA MÁQUINA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/SdGuFWsi4Wg'},
  {nome:'REMADA CURVADA FECHADA NA MÁQUINA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/CyFwxuxdD1Y'},
  {nome:'REMADA ABERTA COM APOIO NA MÁQUINA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/xxhfjVcemrk'},
  {nome:'REMADA FECHADA COM APOIO NA MÁQUINA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/Krg5OX5fTXA'},
  {nome:'REMADA ABERTA PRONADA NA MÁQUINA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/2ZLbfQoVBas'},
  {nome:'PUXADA FRONTAL ABERTA ARTICULADA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/jnSfxuvJ6Lg'},
  {nome:'ABDUÇÃO DE QUADRIL NA POLIA COM ROTAÇÃO - YOUTUBE', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/VrSHOnWrlYQ'},
  {nome:'EXTENSÃO DE QUADRIL COM APOIO NO BANCO - YOUTUBE', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/HmcyJwfM92w'},
  {nome:'EXTENSÃO DE QUADRIL NA POLIA COM TRONCO INCLINADO', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/_Z19zMD6ams'},
  {nome:'REMADA FECHADA NA MAQUINA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/4DyI2sMROzQ'},
  {nome:'POLICHINELO (variação)', grupo:'Aeróbico', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/RraVgclk9UM'},
  {nome:'TRÍCEPS FRANCÊS UNILATERAL', grupo:'Tríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/Nolk5Rqh5SI'},
  {nome:'BOM DIA EM CASA', grupo:'Isquiotibiais', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/ZG44uKbHK4Q'},
  {nome:'STIFF UNILATERAL EM CASA', grupo:'Isquiotibiais', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/6oPT1WncH8U'},
  {nome:'GLÚTEO 4 APOIOS JOELHO FLETIDO EM CASA', grupo:'Glúteos', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/4M4f8ZBLFkA'},
  {nome:'GLÚTEO 4 APOIOS JOELHO ESTENDIDO EM CASA', grupo:'Glúteos', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/xb_lnHwwZE4'},
  {nome:'CRUCIFIXO DEITADO EM CASA', grupo:'Peito', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/Zu_EiENKB-s'},
  {nome:'SUPINO RETO COM BARRA EM CASA', grupo:'Peito', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/4HRd2iAIZgQ'},
  {nome:'TRÍCEPS SUPINADO EM CASA', grupo:'Tríceps', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/UNBGmEOLDtA'},
  {nome:'TRÍCEPS SUPINADO EM CASA', grupo:'Tríceps', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/UNBGmEOLDtA'},
  {nome:'TRÍCEPS TESTA EM CASA', grupo:'Tríceps', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/bpp_l5Gdv7c'},
  {nome:'TRÍCEPS FRANCÊS BILATERAL EM CASA', grupo:'Tríceps', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/rT5dEQzyZy8'},
  {nome:'TRÍCEPS FRANCÊS UNILATERAL EM CASA', grupo:'Tríceps', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/ZgC9-xF_5Zw'},
  {nome:'FLEXÃO NO SOLO EM CASA', grupo:'Peito', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/PD8IJVrCIF0'},
  {nome:'REMADA ABERTA CURVADA COM BARRA EM CASA', grupo:'Costas', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/4rIMCTLueQA'},
  {nome:'ABDUÇÃO DE QUADRIL CONTRA O SOLO EM CASA', grupo:'Glúteos', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/tTwLVwhe8Bg'},
  {nome:'BURP ADAPTADO!! EM CASA', grupo:'Abdômen', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/CtnnoxhzD7U'},
  {nome:'EXTENSÃO DE OMBROS EM CASA', grupo:'Ombros', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/wJCjio_nhjk'},
  {nome:'AGACHAMENTO FRONTAL EM CASA', grupo:'Quadríceps', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/oMoGcwdus3w'},
  {nome:'CADEIRINHA EM CASA', grupo:'Quadríceps', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/BOHl-YMWFnk'},
  {nome:'EXERCÍCIO PASSADA EM CASA', grupo:'Quadríceps', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/XpUCJjVJloI'},
  {nome:'ELEVAÇÃO FRONTAL PEGADA NEUTRA EM CASA', grupo:'Ombros', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/2KOu-qMUffQ'},
  {nome:'CRUCIFIXO INVERSO COM PESO EM CASA', grupo:'Peito', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/mtwV9vh1Yrc'},
  {nome:'REMADA ABERTA COM PESO EM CASA', grupo:'Costas', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/HOD-Um14gFg'},
  {nome:'REMADA NEUTRA CURVADA EM CASA', grupo:'Costas', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/mdtC-9_avxY'},
  {nome:'REMADA NEUTRA CURVADA EM CASA', grupo:'Costas', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/mdtC-9_avxY'},
  {nome:'ELEVAÇÃO FRONTAL COM BARRA EM CASA', grupo:'Ombros', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/dVpeHSXqbis'},
  {nome:'ELEVAÇÃO LATERAL EM CASA', grupo:'Ombros', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/7CpesOm6Z5U'},
  {nome:'DESENVOLVIMENTO FRONTAL EM CASA', grupo:'Ombros', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/Giue8EVBUdw'},
  {nome:'SKIPPING EM CASA', grupo:'Aeróbico', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/6F0ModFWzIg'},
  {nome:'POLICHINELO EM CASA', grupo:'Aeróbico', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/6AIlJkFNrKA'},
  {nome:'ROSCA BÍCEPS COM BARRA EM CASA', grupo:'Bíceps', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/NpnW6BzuNkE'},
  {nome:'ROSCA BÍCEPS COM BARRA EM CASA', grupo:'Bíceps', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/NpnW6BzuNkE'},
  {nome:'ROSCA BÍCEPS ALTERNADA EM CASA', grupo:'Bíceps', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/VbneTGKw5pE'},
  {nome:'ELEVAÇÃO FRONTAL ALTERNADA EM CASA', grupo:'Ombros', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/kGQP3CIow4s'},
  {nome:'ELEVAÇÃO DIAGONAL EM CASA', grupo:'Ombros', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/7_9oQNFkqU0'},
  {nome:'ELEVAÇÃO DIAGONAL EM CASA', grupo:'Ombros', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://www.youtube.com/shorts/7_9oQNFkqU0'},
  {nome:'EXTENSÃO HORIZONTAL DE OMBROS COM ELÁSTICO', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'Elástico', video:'https://www.youtube.com/shorts/aVfjcMPZkJ4'},
  {nome:'REMADA ABERTA ELÁSTICO', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'Elástico', video:'https://www.youtube.com/shorts/soytMrqBEBs'},
  {nome:'PULLDOWN ELÁSTICO', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'Elástico', video:'https://www.youtube.com/shorts/Zg9L-zYE0wI'},
  {nome:'ROTAÇÃO EXTERNA DE OMBROS ELÁSTICO', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'Elástico', video:'https://www.youtube.com/shorts/n6f8dVdeywM'},
  {nome:'EXTENSÃO HORIZONTAL OMBRO ELÁSTICO', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'Elástico', video:'https://www.youtube.com/shorts/mrNPUA1dmsc'},
  {nome:'FLEXÃO COTOVELO ELÁSTICO', grupo:'Bíceps', ambiente:'Academia', nivel:'A definir', metodo:'Elástico', video:'https://www.youtube.com/shorts/dlGMepDtjWg'},
  {nome:'CRUCIFIXO INVERSO CM ELÁSTICO', grupo:'Peito', ambiente:'Academia', nivel:'A definir', metodo:'Elástico', video:'https://www.youtube.com/'},
  {nome:'REMADA CURVADA ABERTA ELÁSTICO', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'Elástico', video:'https://www.youtube.com/shorts/ZCzFQX8LDJM'},
  {nome:'ABDUÇÃO DE QUADRIL COM ELÁSTICO', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'Elástico', video:'https://www.youtube.com/shorts/me5FxLpf2LY'},
  {nome:'FLEXÃO NÓRDICA REVERSA', grupo:'Quadríceps', categoria:'Quadríceps', ambiente:'Academia', nivel:'Avançado', metodo:'Nenhum', video:''},
  {nome:'FLEXÃO NO SOLO ADAPTADA', grupo:'Peito', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/uY3Y9eF79iw'},
  {nome:'CRUCIFIXO INVERSO', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/mtwV9vh1Yrc'},
  {nome:'ELEVAÇÃO FRONTAL ALTERNADA', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/kGQP3CIow4s'},
  {nome:'ROSCA MARTELO COM CORDA NA POLIA', grupo:'Bíceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/KMpBtSwNRcw'},
  {nome:'TRÍCEPS FRANCÊS BIL', grupo:'Tríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/rT5dEQzyZy8'},
  {nome:'PRANCHA VENTRAL DINÂMICA OMBRO', grupo:'Abdômen', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/jSqggWVyTVA'},
  {nome:'FLEXÃO ADAPTADA NO SOFÁ', grupo:'Peito', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/DcOgOamrNVQ'},
  {nome:'CRUCIFIXO INVERSO C PESO', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/mtwV9vh1Yrc'},
  {nome:'DESENVOLVIMENTO FRONTAL', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/Giue8EVBUdw'},
  {nome:'FLEXÃO DE COTOVELO ELÁSTICO', grupo:'Bíceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/dlGMepDtjWg'},
  {nome:'TRÍCEPS FRANCÊS APOIADO', grupo:'Tríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/1XNQYUWWr4w'},
  {nome:'PRANCHA DINÂMICA', grupo:'Abdômen', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/SgSRuUNSaSg'},
  {nome:'SUPINO RETO COM BARRA', grupo:'Peito', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/kwBj4YYcHN0'},
  {nome:'CRUCIFIXO INVERSO COM ELÁSTICO', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/O5Tq-JCRTLU'},
  {nome:'ROTAÇÃO EXTERNA OBROS ELÁSTICO', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/n6f8dVdeywM'},
  {nome:'ROSCA DIRETA COM BARRA W', grupo:'Bíceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/DP_r0Y3jaDQ'},
  {nome:'TRÍCEPS COM CORDA NA POLIA', grupo:'Tríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/5Pi0xpkSJ7I'},
  {nome:'ABS BICICLETA ALT', grupo:'Abdômen', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/ZowzFb-NlVc'},
  {nome:'FLEXÃO DIAGONAL NA POLIA', grupo:'Peito', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/_g-x6FKny3U'},
  {nome:'CRUCIFIXO INVERSO COM HALTERES', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/Qgi26Oy7Izk'},
  {nome:'DESENVOLVIMENTO FRONTAL GUIADA', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/n0sRwlEbFRY'},
  {nome:'ROSCA DIRETA COM HALTERES', grupo:'Bíceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/8UyF5zKDyiw'},
  {nome:'TRÍCEPS FRA UNIL', grupo:'Tríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/ZgC9-xF_5Zw'},
  {nome:'FLEXÃO NO SOLO', grupo:'Peito', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/PD8IJVrCIF0'},
  {nome:'ENCOLHIMENTO', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/AprTVnp8Glc'},
  {nome:'EXTENSÃO DE OMBROS', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/86tIbUP2QDQ'},
  {nome:'ROSCA MARTELO ALTERNADA COM HALTERES', grupo:'Bíceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/-FFDmkFHLNI'},
  {nome:'TRÍCEPS CORDA ELÁSTICO', grupo:'Tríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/8k42fwZDZQk'},
  {nome:'ABS FLEXÃO ALTERNADA E CURTA QUADRIL', grupo:'Abdômen', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/48duQzz6s3o'},
  {nome:'CRUCIFIXO DEITADO', grupo:'Peito', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/Zu_EiENKB-s'},
  {nome:'EXTENSÃO HORIZONTAL VOADOR', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/xRvxuj-CYJU'},
  {nome:'ELEVAÇÃO FRONTAL COM HALTERS', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/8qBPQ9II95g'},
  {nome:'ROSCA DIRETA COM CORDA NA POLIA', grupo:'Bíceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/Zvco9SHfvb4'},
  {nome:'TRÍCEPS CORDA N POLIA', grupo:'Tríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/8QoVDfqOfT0'},
  {nome:'PRANCHA VENTRAL', grupo:'Abdômen', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/BDwijwkx3IM'},
  {nome:'VOADOR', grupo:'Peito', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/Xf-gNOsEAkk'},
  {nome:'FACEPULL ELÁSTICO', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/nEEC350vjtM'},
  {nome:'ELEV LAT. DECLINADA POLIA', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/RKZcUyuDXB4'},
  {nome:'ROSCA SCOTT UNILATERAL', grupo:'Bíceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/eMuzfwJLMHA'},
  {nome:'TRÍCEPS APOIADO', grupo:'Tríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/1XNQYUWWr4w'},
  {nome:'ABS TESOURINHA', grupo:'Abdômen', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/ZamgsrcklEU'},
  {nome:'FACEPULL NA POLIA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/nyD-Hl3ReJI'},
  {nome:'MANGUITO ROT. EXTERNA', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/MizDY8p0gDY'},
  {nome:'ROSCA ALTERNADA COM HALTERES', grupo:'Bíceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/Tzz8CUv-LNk'},
  {nome:'TRÍCEPS FRANCÊS ELÁSTICO', grupo:'Tríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/y9nmjsylNtk'},
  {nome:'ABS REMADOR CURTO C', grupo:'Abdômen', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/FAvPpXN-UrQ'},
  {nome:'DESENVOLVIMENTO FRONTAL HALTERES', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/NwAXR-vZc4Y'},
  {nome:'ROSCA MARTELO COM CORDA', grupo:'Bíceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/81XRt_q084Y'},
  {nome:'TRÍCEPS SUPINADO', grupo:'Tríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/uEUz0NsQNjY'},
  {nome:'SOBE E DESCE CADEIRA', grupo:'Abdômen', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/Nl9Ehzqqq6Q'},
  {nome:'FLEXÃO FECHADA NO SOLO', grupo:'Peito', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/hDbCcR_vCnY'},
  {nome:'PULLDOWN COM ELÁSTICO', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/Zg9L-zYE0wI'},
  {nome:'ROSCA MARTELO COM HALTERES', grupo:'Bíceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/XX8i5RD3XCQ'},
  {nome:'TRÍCEPS COICE UNI NA POLIA', grupo:'Tríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/cQ7TeC_rdlg'},
  {nome:'PRANCHA ALTA', grupo:'Abdômen', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/1L3sIFCFunU'},
  {nome:'PULLDOWN COM CORDA NA POLIA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=H7jlJ-QxC9I'},
  {nome:'ELEV LAT. INCLINADA POLIA', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/IVqSmYZ2awM'},
  {nome:'ROSCA MARTELO ALTERNADA', grupo:'Bíceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/CjfIwcLz5Xk'},
  {nome:'TRÍCEPS  UNI NA POLIA', grupo:'Tríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/udH-RZ2Wb2Y'},
  {nome:'ABS OBLÍCUO TOCANDO O PÉ', grupo:'Abdômen', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/7Wv15aDEhVc'},
  {nome:'PULLDOWN ELÁSTICO', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/Zg9L-zYE0wI'},
  {nome:'ROSCA ALTERNADA COM ISOMETRIA', grupo:'Bíceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/43EPuTFKvjQ'},
  {nome:'TRÍCEPS ROLDANA COM BARRA', grupo:'Tríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/Q5HfQ30Q9os'},
  {nome:'ABS QUADRIL CONTRA O SOLO', grupo:'Abdômen', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/tTwLVwhe8Bg'},
  {nome:'FLEXÃO DE BRAÇOS ADP', grupo:'Peito', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/At33Ihk33pY'},
  {nome:'PUXADA ABERTA FRONTAL Y', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/SBk4080mWa4'},
  {nome:'ELEVAÇÃO LATERAL COM HALTERES', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/13giDFqI39I'},
  {nome:'FLEXÃO DE COTOVELO COM MOCHILA', grupo:'Bíceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/X42BMTwuPMA'},
  {nome:'ABS ESCALADA LATERAL', grupo:'Abdômen', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/rLAnLALPjjE'},
  {nome:'PUXADA COM TRIÂNGULO', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/Ps3_hCKtZ58'},
  {nome:'ELEVAÇÃO LATERAL POLIA', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/w9ebX7LO9SI'},
  {nome:'FLEXÃO COTOVELOS ROLDANA', grupo:'Bíceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/oOPtQW5lubk'},
  {nome:'TRÍCEPS TESTA', grupo:'Tríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/bpp_l5Gdv7c'},
  {nome:'PRANCHA LATERAL', grupo:'Abdômen', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/n9uoOzblcqk'},
  {nome:'SUPINO INCLINADO COM HALTERES', grupo:'Peito', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/aCY-5vtz3n8'},
  {nome:'PUXADA FRONTAL BARRA FIXA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/Ds5iplZZf9Q'},
  {nome:'MANGUITO ELÁSTICO', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/uNHCiK5oARI'},
  {nome:'ROSCA SCOTT', grupo:'Bíceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/hFb3-9uXdHo'},
  {nome:'TRÍCEPS BANCO EM CASA', grupo:'Tríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/wkaPvUI9VBg'},
  {nome:'ABS SUPRA', grupo:'Abdômen', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/SEaT1STUVF8'},
  {nome:'CRUCIFIXO COM HALTERES', grupo:'Peito', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/t5LWZvJErcA'},
  {nome:'PUXADA ABERTA SUPINADA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/-7QIrHHcIO4'},
  {nome:'EXTENSÃO HORIZONTAL DE OMBROS', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/aVfjcMPZkJ4'},
  {nome:'ROSCA BÍCEPS ALTERNADA', grupo:'Bíceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/VbneTGKw5pE'},
  {nome:'TRÍCEPS CORDA', grupo:'Tríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/V8OOh_Od_0o'},
  {nome:'ABS INFRA', grupo:'Abdômen', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/ujwvd_T1R0c'},
  {nome:'CROSSOVER DIAGONAL', grupo:'Peito', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/_g-x6FKny3U'},
  {nome:'PUXADA FRONTAL SUPINADA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/VyIVo-I0rVo'},
  {nome:'EXT HORIZONTAL ELÁSTICO', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/mrNPUA1dmsc'},
  {nome:'TRÍCEPS COM BARRA NA POLIA', grupo:'Tríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/lglZg5PueKE'},
  {nome:'ABS INFRA CURTO', grupo:'Abdômen', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/1bfJ7vBi_Tk'},
  {nome:'SUPINO RETO COM HALTERES', grupo:'Peito', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/Z4GP4FWN9YM'},
  {nome:'PUXADA NEUTRA BARRA FIXA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/zYNDXu3HxEw'},
  {nome:'ELEVAÇÃO LATERAL ELÁSTICO', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/zZXRdoICDzY'},
  {nome:'ROSCA MARTELO NA POLIA', grupo:'Bíceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/KMpBtSwNRcw'},
  {nome:'MERGULHO NA PARALELA', grupo:'Tríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/f3h0EtJIIhY'},
  {nome:'ABS ESCALADA', grupo:'Abdômen', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/cV6K_j5FLt8'},
  {nome:'PUXADA NEUTRA FRONTAL', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/TmG8s45O-BI'},
  {nome:'ELEVAÇÃO FRONTAL C BARRA', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/dVpeHSXqbis'},
  {nome:'TRÍCEPS FRANCÊS BILATERAL', grupo:'Tríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/rT5dEQzyZy8'},
  {nome:'ABDOMINAL CANIVETE', grupo:'Abdômen', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/kjoeOZKVxDM'},
  {nome:'REMADA ABE CURV C BARRA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/4rIMCTLueQA'},
  {nome:'ELEVAÇÃO FRONTAL NA POLIA', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/AcbMzWL2E7Q'},
  {nome:'ROSCA DIRETA INCLINADA', grupo:'Bíceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/VnU96uihAJc'},
  {nome:'ABS REMADOR COMPLETO', grupo:'Abdômen', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/cPePOSFPFQQ'},
  {nome:'REMADA ABERTA C/ HALTER', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/HOD-Um14gFg'},
  {nome:'EXTENSÃO HORIZINTAL HALTER', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/54jt9FkDs_w'},
  {nome:'ROSCA 21 COM BARRA', grupo:'Bíceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/l-MktIlWRZg'},
  {nome:'ABS BICICLETA UNIL.', grupo:'Abdômen', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/IQMbbBpLWaM'},
  {nome:'REMADA ABERTA COM HALTERES', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/giQfgYKYrGc'},
  {nome:'ELEVAÇÃO LATERAL HALTERES', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/xaZ1GlPzse4'},
  {nome:'PRANCHA VENTRAL 3 APOIOS', grupo:'Abdômen', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/vJkSlg0__A4'},
  {nome:'REMADA ABERTA ELÁSTICA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/soytMrqBEBs'},
  {nome:'ELEVAÇÃO FRONTAL', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/x6kVaimzO7o'},
  {nome:'REMADA ABERTA FRONTAL ELÁSTICO', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/uuCA8vg7xSQ'},
  {nome:'ELEVAÇÃO FRONTAL HALTERES', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/x6kVaimzO7o'},
  {nome:'ABS CRUZADO ALTO', grupo:'Abdômen', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/b1qXTPHPpjw'},
  {nome:'CROOS OVER', grupo:'Peito', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/_g-x6FKny3U'},
  {nome:'REMADA ABERTA PRONADA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/Je-c5O5DR5c'},
  {nome:'EXTENSÃO HORIZONTAL UNI NA POLIA', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/qCQOT8Py038'},
  {nome:'ABS REMADOR CURTO', grupo:'Abdômen', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/FAvPpXN-UrQ'},
  {nome:'REMADA CURVADA SUPINADA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/3VU_r-9snjg'},
  {nome:'DESENVOLVIMENTO FRONTAL ELÁSTICO', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/6p5_P_8VH-g'},
  {nome:'PRANCHA LATERAL ADAP.', grupo:'Abdômen', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/8l3N6o2SLMM'},
  {nome:'SUPINO INCLINADO HALTERES', grupo:'Peito', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/TXXG_sevBMk'},
  {nome:'REMADA BAIXA NEUTRA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/gvRz36ZV9SQ'},
  {nome:'ELEVAÇÃO LATERAL', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/7CpesOm6Z5U'},
  {nome:'PRANCHA LATERAL DINÂMICA', grupo:'Abdômen', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/Z-V-LAchIn0'},
  {nome:'REMADA BAIXA Y', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/iKW5nXARbMs'},
  {nome:'DESENVOLVIMENTRO FRONTAL COM MOCHILA', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/RcHEOOACjHk'},
  {nome:'REMADA CURVADA ABE ELÁSTICO', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/ZCzFQX8LDJM'},
  {nome:'MANGUITO UNILATERAL', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/286eFOvP_MI'},
  {nome:'TRÍCEPS BARRA NA POLIA', grupo:'Tríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/Byff4QmSEc8'},
  {nome:'SUPINO RETO ARTICULAR', grupo:'Peito', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/kn6zOZXNW1Q'},
  {nome:'REMADA CURVADA ABERTA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/Je-c5O5DR5c'},
  {nome:'ELEVAÇÃO DIAGONAL H', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/7_9oQNFkqU0'},
  {nome:'REMADA CURVADA COM HALTERES', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/ReiU3emM_8o'},
  {nome:'ELEVAÇÃO FRONTAL ELÁSTICO', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/UavSXczA6pI'},
  {nome:'FLY', grupo:'Peito', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/L4YtdisHldk'},
  {nome:'REMADA CURVADA COM MOCHILA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/7nlABnGxDp0'},
  {nome:'EXTENSÃO HORIZONTAL POLIA', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/1MHzijrsAOY'},
  {nome:'SUPINO INCLINADO ARTICULAR', grupo:'Peito', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/oJkLYcqaA5E'},
  {nome:'REMADA CURVADA NEUTRA ELÁSTICO', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/_kI51Xc7tnE'},
  {nome:'REMADA FECHADA CURVADA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/FeIxXCMuwUI'},
  {nome:'REMADA FECHADA ELÁSTICO', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/0xI3hZwo3OA'},
  {nome:'REMADA UNI AMPLITUDE ELEVADA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/_I2d2Ij0p5E'},
  {nome:'MANGUITO COM ELÁSTICO', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/o-53LBfn9ys'},
  {nome:'REMADA UNI COM HALTERES', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/kS0KNWUgU7I'},
  {nome:'REMADA UNILATERAL COM MOCHILA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/i__eZEOAYEE'},
  {nome:'DESENVOLVIMENTO FRONTAL NA MÁQUINA', grupo:'Ombros', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/iT29kcK5KcY'},
  {nome:'EXTENSÃO HORIZONTAL NA POLIA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/1MHzijrsAOY'},
  {nome:'REMADA ABERTA COM LENÇOL', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/DKHa19UU4EU'},
  {nome:'PULLOVER', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/Ba4xQwfBz6g'},
  {nome:'REMADA ARTICULAR ABERTA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/ueWKZBDgQeA'},
  {nome:'REMADA ARTICULAR FECHADA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/Bjy5YMPzZao'},
  {nome:'PUXADA ABERTA FRONTAL', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/rubLhTeJWwA'},
  {nome:'PUXADA SUPINADA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/JROH1Y73n6E'},
  {nome:'REMADA BAIXA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/tGWs8IjRN9s'},
  {nome:'REMADA ABERTA NA POLIA', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/-AhBdO6ngdE'},
  {nome:'CRUCIFIXO INV COM APOIO', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/IbhLDWWnGxc'},
  {nome:'REMADA CAVALINHO', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/MkfpH3Sl1lY'},
  {nome:'REMADA ABERTA NO TRX', grupo:'Costas', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/KoxP8668V1Y'},
  {nome:'TRÍCEPS FRAN BIL', grupo:'Bíceps', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://youtu.be/rT5dEQzyZy8'},
  {nome:'PULLDOWN COM CORDA', grupo:'Costas', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://youtu.be/Zg9L-zYE0wI'},
  {nome:'REMADA ABERTA SUPINADA', grupo:'Costas', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://youtu.be/3VU_r-9snjg'},
  {nome:'EXTENSÃO LOMBAR DINÂMICA', grupo:'Bíceps', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://youtu.be/y_5kPLWzrFQ'},
  {nome:'EXTENSÃO LOMBAR GUIADO', grupo:'Bíceps', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://youtu.be/M7wFYedPnLc'},
  {nome:'SUPER MAN', grupo:'Bíceps', ambiente:'Casa', nivel:'A definir', metodo:'', video:'https://youtu.be/lhg93GRGkGQ'},
  {nome:'AFUNDO', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/JM5sJCb1lsI'},
  {nome:'BOM DIA', grupo:'Isquiotibiais', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/ZG44uKbHK4Q'},
  {nome:'ABDUÇÃO DE QUADRIL NA POLIA', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/NOkHTTQNqSk'},
  {nome:'FLEXÃO PLANTAR', grupo:'Panturrilha', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/bc12xKf4-9A'},
  {nome:'AFUNDO ALT. PLIOMÉTRICO', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/1tY7QZwhpGU'},
  {nome:'CADEIRA FLEXORA', grupo:'Isquiotibiais', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/Ve2BanEgCqY'},
  {nome:'ABDUÇÃO DE QUADRIL ELÁSTICO', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/e0M1vNL15sE'},
  {nome:'FLEXÃO PLANTAR NA MÁQUINA', grupo:'Panturrilha', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/bs3NdonHzhs'},
  {nome:'AFUNDO ALTERNADO', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/oY_nnRig9J4'},
  {nome:'EDUCATIVO STIFF', grupo:'Isquiotibiais', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/1P03JzjPPOE'},
  {nome:'CADEIRA ABDUTORA( ABRIR )', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/WMx7JeUTXJs'},
  {nome:'AFUNDO C/FLEX DE QUADRIL', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/OMQhwRW23ZI'},
  {nome:'ELEVAÇÃO FROG', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/j4_-w9BcarQ'},
  {nome:'CADEIRA ADUTORA ( FECHAR )', grupo:'Panturrilha', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/lzuSEi65_IQ'},
  {nome:'AFUNDO COM ELÁSTICO', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/YWvy-2OLeJU'},
  {nome:'ELEVAÇÃO PÉLVICA', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/O84nvpikZiU'},
  {nome:'AFUNDO DIN. C FLEX QUADRIL', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/OMQhwRW23ZI'},
  {nome:'ELEVAÇÃO PÉLVICA UNILATERAL', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/WM4Blto4Uq0'},
  {nome:'AFUNDO GUIADO', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/xMTJPIEVLpo'},
  {nome:'STIFF COM ELÁSTICO', grupo:'Isquiotibiais', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/wzeYDYTKwp0'},
  {nome:'EXTENSÃO DE QUADRIL', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/uhfO-KWURBE'},
  {nome:'AFUNDO NA BARRA GUIADA', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/EcvoioUwyH8'},
  {nome:'EXTENSÃO DE QUADRIL ELÁSTICO', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/e_Kf5MpbkUo'},
  {nome:'AGACHAMENTO BÚLGARO', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/GGvEWR0Y4Wc'},
  {nome:'STIFF UNILATERAL', grupo:'Isquiotibiais', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/6oPT1WncH8U'},
  {nome:'AGACHAMENTO FRONTAL', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/oMoGcwdus3w'},
  {nome:'CAMA FLEXORA', grupo:'Isquiotibiais', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/ByKiWoif3iA'},
  {nome:'EXTENSÃO DE QUADRIL VARIAÇÃO', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/2lb69Bohm_8'},
  {nome:'AGACHAMENTO GUIADO P', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/3SXMi9dGQkM'},
  {nome:'FLEXÕ NÓRDICA', grupo:'Isquiotibiais', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/DIRx__DoJqo'},
  {nome:'FLEX. DE JOELHOS NO SOLO', grupo:'Isquiotibiais', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/fWW4ASUCi_c'},
  {nome:'AGACHAMENTO GUIADO Y', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/ajf2fUekZr4'},
  {nome:'FLEXÃO DE JOELHOS COM BOLA ADAPTADO', grupo:'Isquiotibiais', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/DgDD-47bVGk'},
  {nome:'FLEX. DE JOELHOS DE PÉ', grupo:'Isquiotibiais', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/-ixMlKa11bA'},
  {nome:'AGACHAMENTO HACK', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/-qYxx4mRjoc'},
  {nome:'STIFF COM HALTERES', grupo:'Isquiotibiais', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/5rJy8qqQofQ'},
  {nome:'FLEX. JOELHOS ADP', grupo:'Isquiotibiais', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/6bE5P4x36-o'},
  {nome:'AGACHAMENTO HACK Y', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/hqTvBjX4lgY'},
  {nome:'STIFF UNI SEM APOIO', grupo:'Isquiotibiais', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/E6QF1VWS1qY'},
  {nome:'FLEXÃO DE JOELHOS COM BOLA', grupo:'Isquiotibiais', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/H0nTtztJK8M'},
  {nome:'AGACHAMENTO LIVRE', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/3Mq1lLgE00g'},
  {nome:'STIFF UNI COM APOIO', grupo:'Isquiotibiais', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/GyDrpYEPWQw'},
  {nome:'FROG NO SOLO', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/tWdgdM3B5rA'},
  {nome:'AGACHAMENTO PC', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/pcfm6MBXeMg'},
  {nome:'FLEXÃO DE JOELHOS EM PÉ', grupo:'Isquiotibiais', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/7H6O9-WYZ74'},
  {nome:'GLÚT. 4 APOIOS JOE. ESTENDIDO', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/xb_lnHwwZE4'},
  {nome:'AGACHAMENTO PLIOM. ALTERNADO', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/1tY7QZwhpGU'},
  {nome:'GLÚT. 4 APOIOS JOE. FLETIDO', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/4M4f8ZBLFkA'},
  {nome:'AGACHAMENTO PLIOMÉTRICO', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/jg5y0pyv3Xo'},
  {nome:'STIFF', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/D5sRfuzL0e4'},
  {nome:'AGACHAMENTO SUMÔ COM PESO', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/l_t_84gmZQQ'},
  {nome:'TERRA ROMENO', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/VpjkLAOz8CU'},
  {nome:'AGACHAMENTO SUMÔ', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/4Gm4RDvDSNk'},
  {nome:'TERRA ISOLADO', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/E7CZQS-PRoo'},
  {nome:'AGACHAMENTO SUMÔ ELÁSTICO', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/ubzfIt524k8'},
  {nome:'LEVANTAMENTO TERRA', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/EscCinkCoBI'},
  {nome:'AJOELHA E LEVANTA', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/0Zgh3NIsMfc'},
  {nome:'ABDUÇÃO QUADRIL DEITADO', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/yBAkhqWLmYE'},
  {nome:'AVANÇO', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/l957uDIyzX0'},
  {nome:'ELEVAÇÃO PÉLVICA COM BARRA', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/bkLj_jbodUE'},
  {nome:'BURP ADAPTADO', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/CtnnoxhzD7U'},
  {nome:'ABD QUADRIL SENTADO ELÁSTICO', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/58bn5r9XEcI'},
  {nome:'CADEIRA EXTENSORA', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/Ox4ZtBUAGo4'},
  {nome:'ABDUÇÃO QUADRIL NO SOLO', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=tTwLVwhe8Bg'},
  {nome:'CADEIRA EXTENSORA UNI', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/HRjzMl7a-IY'},
  {nome:'Glúteo 4 apoios com caneleiras', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/JmnIUWP5gno'},
  {nome:'CADEIRINHA', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/BOHl-YMWFnk'},
  {nome:'EDUCATIVO AGACHAMENTO', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/WP-gDofgT8c'},
  {nome:'Extensão de quadril  na polia com tronco inclinado', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/dvsgo4Gj-hA'},
  {nome:'FLEXÃO DE QUADRIL DEITADO', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/GJ4DAuWxuVU'},
  {nome:'Extensão de quadril no banco romano', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/6ODl8JFyyYA'},
  {nome:'FLEXÃO DE QUADRIL EM PÉ', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/Cj7FbWvklPE'},
  {nome:'Abdução de quadril na polia com tronco inclinado', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtube.com/shorts/MpTUBFeZya8'},
  {nome:'LEG PRESS 45', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/HHYpoJ1l4Z0'},
  {nome:'Abdução de quadril na polia com rotação', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtube.com/shorts/HmcyJwfM92w'},
  {nome:'LEG PRESS 45 Y', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/V5Y0w8nbmdU'},
  {nome:'LEG PRESS HORIZONTAL', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/0KLrU6JXOUE'},
  {nome:'CADEIRA ABDUTORA', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/L0L9s4OTbOU'},
  {nome:'LEVANTAMENO TERRA EM CASA', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/Hgas-jdhnEQ'},
  {nome:'CADEIRA ABDUTORA-TRONCO A FRENTE', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/aKzeShloHJs'},
  {nome:'LEVANTAMENTO TERRA COM BARRA', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/EscCinkCoBI'},
  {nome:'PASSADA COM HALTERES', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/1sbNNUMstns'},
  {nome:'extensão de quadril na máquina - variação', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/IWpwUAgwnpI'},
  {nome:'PASSADA LIVRE', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/4UIJPNFNHG0'},
  {nome:'EXTENSÃO DE QUADRIL NA MÁQUINA', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/0jzdxWcaT5I'},
  {nome:'SENTA E LEVANTA', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/iqK9m1CLnQE'},
  {nome:'EXTENSÃO DE QUADRIL COM CANELEIRAS NO BANCO', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/5BuJFrdn_AY'},
  {nome:'ELEVAÇÃO PÉLVICA NA MÁQUINA', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/K1QVmQb59rI'},
  {nome:'TERRA ROMÊNO ELÁSTICO', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/3kVNP8g0xmY'},
  {nome:'extensão de quadril curtinha no alto', grupo:'Glúteos', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/QXrJE4FF-TM'},
  {nome:'TERRA SUMÔ ELÁSTICO', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/k7O5CZGwrh4'},
  {nome:'TERRA SUMÔ Y', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/ofnXsX-8aU8'},
  {nome:'AGACHAMENTO SQUAT', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://www.youtube.com/watch?v=I3RAHGIS92I'},
  {nome:'AGACHAMENTO BÚLGARO COM PESO CONTRALATERAL', grupo:'Quadríceps', ambiente:'Academia', nivel:'A definir', metodo:'', video:'https://youtu.be/RSULENERtug'}
];

// Blindagem permanente: remove qualquer item que tenha entrado por engano no banco de exercícios
// mas que na verdade é vídeo de ajuste/correção/dica, não um exercício de treino de verdade.
// Roda toda vez que a página carrega, então protege mesmo se alguém adicionar errado no futuro.
for(let __i = exerciciosBanco.length - 1; __i >= 0; __i--){
  const __nomeUpper = exerciciosBanco[__i].nome.toUpperCase();
  if(palavrasNaoSaoExercicio.some(function(p){ return __nomeUpper.indexOf(p) !== -1; })){
    exerciciosBanco.splice(__i, 1);
  }
}

const categoriasMusculo = ['Quadríceps', 'Posterior de Coxa', 'Glúteos', 'Panturrilha', 'Abdômen', 'Peito', 'Ombros', 'Costas', 'Bíceps', 'Tríceps', 'Sem categoria'];

const mapaGrupoOriginal = {
  'Quadríceps': 'Quadríceps', 'Isquiotibiais': 'Posterior de Coxa', 'Glúteos': 'Glúteos', 'Panturrilha': 'Panturrilha',
  'Abdômen': 'Abdômen', 'Peito': 'Peito', 'Ombros': 'Ombros', 'Costas': 'Costas', 'Bíceps': 'Bíceps', 'Tríceps': 'Tríceps'
};
exerciciosBanco.forEach(function(e){
  e.categoria = mapaGrupoOriginal[e.grupo] || 'Sem categoria';
});

let editMode = false;
let filtroExercicio = 'Todos';
let mostrandoFormAdicionar = false;
let paginaAtualExercicios = 1;
let filtroAmbienteCasa = false;
const ITENS_POR_PAGINA_EXERCICIOS = 10;

function renderExerciciosChips(){
  const container = document.getElementById('ex-grupos-lista');
  if(!container) return;
  document.getElementById('ex-count-label').textContent = exerciciosBanco.length + ' exercícios cadastrados';
  container.innerHTML = '';

  categoriasMusculo.forEach(function(cat){
    const qtd = exerciciosBanco.filter(function(e){ return e.categoria === cat; }).length;
    const btn = document.createElement('div');
    btn.className = 'list-item';
    btn.style.cursor = 'pointer';
    btn.innerHTML = '<span>' + cat + '</span><span class="tag">' + qtd + ' exerc.</span>';
    btn.onclick = function(){ abrirGrupoExercicios(cat); };
    container.appendChild(btn);
  });

  const btnCriar = document.createElement('div');
  btnCriar.className = 'list-item';
  btnCriar.style.cssText = 'cursor:pointer;background:var(--card-2);color:var(--gold-soft);margin-top:10px;';
  btnCriar.innerHTML = '<span><i class="ti ti-plus" style="font-size:14px;vertical-align:-2px;margin-right:6px;"></i>Criar exercício novo</span>';
  btnCriar.onclick = function(){ abrirGrupoExercicios('__criar__'); };
  container.appendChild(btnCriar);

  const btnCasa = document.createElement('div');
  btnCasa.className = 'list-item';
  btnCasa.style.cssText = 'cursor:pointer;background:var(--card-2);color:var(--gold-soft);margin-top:6px;';
  btnCasa.innerHTML = '<span><i class="ti ti-home" style="font-size:14px;vertical-align:-2px;margin-right:6px;"></i>Vídeos de casa</span>';
  btnCasa.onclick = function(){ abrirGrupoExercicios('Casa'); };
  container.appendChild(btnCasa);
}

function abrirGrupoExercicios(cat){
  filtroExercicio = cat === 'Casa' ? 'Todos' : cat;
  filtroAmbienteCasa = cat === 'Casa';
  paginaAtualExercicios = 1;
  mostrandoFormAdicionar = (cat === '__criar__');
  if(mostrandoFormAdicionar) filtroExercicio = 'Todos';
  document.getElementById('ex-grupos-view').style.display = 'none';
  document.getElementById('ex-lista-view').style.display = 'block';
  document.getElementById('ex-grupo-titulo').textContent = cat === '__criar__' ? 'Criar exercício novo' : (cat === 'Casa' ? 'Vídeos de casa' : cat);
  if(mostrandoFormAdicionar){
    populateGrupoSelect();
    document.getElementById('ex-add-form').style.display = 'block';
    document.getElementById('ex-lista').style.display = 'none';
    document.getElementById('ex-paginacao').innerHTML = '';
  } else {
    renderExerciciosLista();
  }
}

function voltarParaGruposExercicios(){
  document.getElementById('ex-lista-view').style.display = 'none';
  document.getElementById('ex-grupos-view').style.display = 'block';
  filtroAmbienteCasa = false;
  renderExerciciosChips();
}

let termoBuscaExercicios = '';

function buscarExerciciosNoGrupo(termo){
  termoBuscaExercicios = termo;
  paginaAtualExercicios = 1;
  renderExerciciosLista();
}

function renderExerciciosLista(){
  document.getElementById('ex-add-form').style.display = 'none';
  const list = document.getElementById('ex-lista');
  list.style.display = 'block';
  list.innerHTML = '';
  let itens = filtroExercicio === 'Todos' ? exerciciosBanco.slice() : exerciciosBanco.filter(function(e){ return e.categoria === filtroExercicio; });
  if(filtroAmbienteCasa) itens = itens.filter(function(e){ return e.ambiente === 'Casa'; });
  if(termoBuscaExercicios.trim()){
    const termoUpper = termoBuscaExercicios.trim().toUpperCase();
    itens = itens.filter(function(e){ return e.nome.toUpperCase().indexOf(termoUpper) !== -1; });
  }

  if(itens.length === 0){
    list.innerHTML = '<div class="info-box"><p class="txt">Nenhum exercício encontrado' + (termoBuscaExercicios.trim() ? ' com esse nome' : ' aqui ainda') + '.</p></div>';
    document.getElementById('ex-paginacao').innerHTML = '';
    return;
  }

  const totalPaginas = Math.max(1, Math.ceil(itens.length / ITENS_POR_PAGINA_EXERCICIOS));
  if(paginaAtualExercicios > totalPaginas) paginaAtualExercicios = totalPaginas;
  const inicio = (paginaAtualExercicios - 1) * ITENS_POR_PAGINA_EXERCICIOS;
  const itensDaPagina = itens.slice(inicio, inicio + ITENS_POR_PAGINA_EXERCICIOS);

  itensDaPagina.forEach(function(e){
    const el = document.createElement('div');
    el.className = 'exercicio-item';
    const videoHtml = e.video
      ? '<span class="ex-video ok"><i class="ti ti-circle-check" style="font-size:11px;vertical-align:-1px;margin-right:3px;"></i>Assistir no app</span>'
      : '<span class="ex-video pending"><i class="ti ti-clock" style="font-size:11px;vertical-align:-1px;margin-right:3px;"></i>Sem vídeo ainda</span>';

    if(editMode){
      el.style.cursor = 'default';
      el.style.cssText += 'flex-direction:column;align-items:stretch;gap:6px;';
      const idSeguro = e.nome.replace(/[^a-zA-Z0-9]/g,'');
      el.innerHTML =
        '<div class="form-group" style="margin-bottom:4px;"><label class="form-label">Nome do exercício</label><input class="form-input" id="edit-nome-' + idSeguro + '" value="' + e.nome.replace(/"/g,'&quot;') + '" style="font-size:13px;padding:8px;"></div>' +
        '<div class="form-group" style="margin-bottom:4px;"><label class="form-label">Link do vídeo</label><input class="form-input" id="edit-video-' + idSeguro + '" value="' + (e.video || '').replace(/"/g,'&quot;') + '" placeholder="Cole o link do YouTube" style="font-size:13px;padding:8px;"></div>' +
        videoHtml;

      const seletorMover = document.createElement('select');
      seletorMover.className = 'form-select';
      seletorMover.style.cssText = 'font-size:11px;padding:4px 6px;margin-top:6px;width:auto;';
      seletorMover.innerHTML = '<option value="">Mover pra outro grupo...</option>' +
        categoriasMusculo.filter(function(c){ return c !== e.categoria; }).map(function(c){ return '<option value="' + c + '">' + c + '</option>'; }).join('');
      seletorMover.onchange = function(){
        if(!seletorMover.value) return;
        e.categoria = seletorMover.value;
        e.grupo = seletorMover.value;
        salvarCatalogoPersonal('exercicio_movido', e.nome, { nome: e.nome, categoria: e.categoria });
        renderExerciciosLista();
      };
      el.appendChild(seletorMover);

      const btnSalvar = document.createElement('span');
      btnSalvar.className = 'acao-pill';
      btnSalvar.style.cssText = 'display:inline-block;margin-top:6px;margin-left:6px;';
      btnSalvar.textContent = 'Salvar alterações';
      btnSalvar.onclick = function(){
        const novoNome = document.getElementById('edit-nome-' + idSeguro).value.trim();
        const novoVideo = document.getElementById('edit-video-' + idSeguro).value.trim();
        if(!novoNome){ alert('O nome não pode ficar vazio.'); return; }
        const nomeAntigo = e.nome;
        e.nome = novoNome;
        e.video = novoVideo;
        salvarCatalogoPersonal('exercicio_editado', nomeAntigo, { nomeAntigo: nomeAntigo, nome: e.nome, video: e.video, categoria: e.categoria, grupo: e.grupo });
        renderExerciciosLista();
      };
      el.appendChild(btnSalvar);

      const btnExcluir = document.createElement('span');
      btnExcluir.className = 'acao-pill destrutiva';
      btnExcluir.style.cssText = 'display:inline-block;margin-top:6px;margin-left:6px;';
      btnExcluir.textContent = 'Excluir';
      btnExcluir.onclick = function(){ excluirExercicioBanco(e.nome); };
      el.appendChild(btnExcluir);
    } else {
      el.style.cursor = 'pointer';
      el.innerHTML =
        '<p class="ex-name">' + e.nome + '</p>' +
        '<p class="ex-meta">' + e.categoria + (e.ambiente ? ' · ' + e.ambiente : '') + (e.nivel ? ' · ' + e.nivel : '') + '</p>' +
        videoHtml;
      el.onclick = function(){ playExercicioVideo(e.nome); };
    }
    list.appendChild(el);
  });

  const linkReorganizar = document.createElement('p');
  linkReorganizar.style.cssText = 'font-size:11px;color:var(--text-faint);text-align:center;margin-top:14px;cursor:pointer;';
  linkReorganizar.textContent = editMode ? 'Concluir reorganização' : 'Reorganizar exercícios entre grupos';
  linkReorganizar.onclick = toggleEditMode;
  list.appendChild(linkReorganizar);

  // Paginação
  const pagArea = document.getElementById('ex-paginacao');
  pagArea.innerHTML = '';
  if(totalPaginas > 1){
    for(let p = 1; p <= totalPaginas; p++){
      const btnPag = document.createElement('div');
      btnPag.className = 'chip' + (p === paginaAtualExercicios ? ' active' : '');
      btnPag.style.cssText = 'min-width:30px;text-align:center;cursor:pointer;';
      btnPag.textContent = p;
      btnPag.onclick = function(){ paginaAtualExercicios = p; renderExerciciosLista(); window.scrollTo(0,0); };
      pagArea.appendChild(btnPag);
    }
  }
}

function playExercicioVideo(nome){
  const e = exerciciosBanco.find(function(x){ return x.nome === nome; });
  if(!e) return;
  const embed = getEmbedUrl(e.video);
  const el = document.getElementById('ex-video-content');
  el.innerHTML =
    (embed
      ? '<div class="video-block"><iframe src="' + embed + '" title="' + e.nome + '" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></div>' + ytFallback(e.video)
      : '<div class="video-block"><i class="ti ti-player-play"></i></div>') +
    '<h1 class="page-title" style="margin-top:0;">' + e.nome + '</h1>' +
    '<p class="page-sub">' + e.categoria + (e.nivel ? ' · ' + e.nivel : '') + '</p>' +
    '<button class="btn-gold" style="background:var(--card-2);color:var(--gold-soft);border:1px solid var(--border);" onclick="abrirEdicaoExercicio(\'' + nome.replace(/'/g,"\\'") + '\')">Ativar modo de edição</button>' +
    '<div id="ex-edicao-form"></div>';
  document.getElementById('ex-lista-view').style.display = 'none';
  document.getElementById('ex-video-view').style.display = 'block';
}

function abrirEdicaoExercicio(nome){
  const e = exerciciosBanco.find(function(x){ return x.nome === nome; });
  if(!e) return;
  const metodos = ['Nenhum','Restpause','Dropset','Cluster set','Bi-set','Tri-set','Pirâmide crescente'];
  const opcoesMetodo = metodos.map(function(m){ return '<option' + (e.metodo === m ? ' selected' : '') + '>' + m + '</option>'; }).join('');
  document.getElementById('ex-edicao-form').innerHTML =
    '<div class="info-box" style="margin-top:10px;">' +
      '<div class="form-group"><label class="form-label">Método padrão (opcional)</label><select class="form-select" id="edicao-metodo">' + opcoesMetodo + '</select></div>' +
      '<div class="form-group"><label class="form-label">Link do vídeo (opcional)</label><input class="form-input" id="edicao-video" value="' + (e.video || '') + '" placeholder="Cole o link"></div>' +
      '<button class="btn-gold" onclick="confirmarEdicaoExercicio(\'' + nome.replace(/'/g,"\\'") + '\')">Confirmar</button>' +
    '</div>';
}

function confirmarEdicaoExercicio(nome){
  const e = exerciciosBanco.find(function(x){ return x.nome === nome; });
  if(!e) return;
  e.metodo = document.getElementById('edicao-metodo').value;
  e.video = document.getElementById('edicao-video').value.trim();
  playExercicioVideo(nome);
}

function toggleEditMode(){
  editMode = !editMode;
  mostrandoFormAdicionar = false;
  renderExerciciosChips();
  renderExerciciosLista();
}

function fecharVideoExercicio(){
  document.getElementById('ex-video-view').style.display = 'none';
  document.getElementById('ex-lista-view').style.display = 'block';
}


// ===== TABATA · protocolo original de Izumi Tabata (1996): 20s trabalho / 10s descanso, 8 ciclos, 4min =====
// Adaptado por nível porque a literatura confirma que o protocolo original não é indicado
// como primeiro contato pra quem está começando (intensidade quase máxima, perto de 170% do VO2máx).
const protocolosTabata = {
  'Iniciante': {
    trabalhoSeg: 20, descansoSeg: 20, ciclos: 6, blocos: 1,
    descricao: 'Trabalho e descanso iguais (1:1), menos ciclos. Prioriza aprender o ritmo com segurança antes de aumentar a intensidade.'
  },
  'Intermediário': {
    trabalhoSeg: 20, descansoSeg: 15, ciclos: 8, blocos: 1,
    descricao: 'Já aproxima do padrão clássico, com um pouco mais de descanso que o protocolo original.'
  },
  'Avançado': {
    trabalhoSeg: 20, descansoSeg: 10, ciclos: 8, blocos: 2,
    descricao: 'Protocolo original de Izumi Tabata (20s/10s, 8 ciclos), em 2 blocos de exercícios diferentes.'
  }
};

// Banco de exercícios de peso corporal, sem equipamento, pensados pra treino de casa em formato Tabata
const bancoTabataExercicios = [
  { nome: 'Polichinelo', restricao: null, impacto: 'alto' },
  { nome: 'Agachamento livre', restricao: null, impacto: 'baixo' },
  { nome: 'Agachamento com salto', restricao: 'joelho', impacto: 'alto' },
  { nome: 'Mountain climber', restricao: 'punho', impacto: 'moderado' },
  { nome: 'Prancha com toque no ombro', restricao: 'punho', impacto: 'baixo' },
  { nome: 'Afundo alternado', restricao: 'joelho', impacto: 'moderado' },
  { nome: 'Elevação de joelhos no lugar (corrida parada)', restricao: null, impacto: 'moderado' },
  { nome: 'Burpee sem salto', restricao: 'joelho', impacto: 'moderado' },
  { nome: 'Abdominal bicicleta', restricao: 'lombar', impacto: 'baixo' },
  { nome: 'Ponte de glúteo', restricao: null, impacto: 'baixo' },
  { nome: 'Chute lateral em pé', restricao: null, impacto: 'baixo' },
  { nome: 'Skater jump (agachamento lateral saltado)', restricao: 'joelho', impacto: 'alto' }
];

function gerarTreinoTabata(nivel, restricoesTexto){
  const protocolo = protocolosTabata[nivel] || protocolosTabata['Iniciante'];
  const restricoesLower = (restricoesTexto || '').toLowerCase();

  // Filtra exercícios que batem com alguma restrição relatada na anamnese, pra não prescrever o que não deveria
  const exerciciosSeguros = bancoTabataExercicios.filter(function(ex){
    if(!ex.restricao) return true;
    return restricoesLower.indexOf(ex.restricao) === -1;
  });

  // Iniciante: evita impacto alto de propósito, mesmo que ela não tenha restrição relatada
  const pool = nivel === 'Iniciante' ? exerciciosSeguros.filter(function(ex){ return ex.impacto !== 'alto'; }) : exerciciosSeguros;
  const poolFinal = pool.length >= 4 ? pool : exerciciosSeguros; // salvaguarda, nunca deixa a lista vazia demais

  // Embaralha e pega exercícios suficientes pra todos os blocos (1 exercício por ciclo dentro de cada bloco, repetindo os 8 ciclos no mesmo exercício, como no protocolo original)
  const embaralhados = poolFinal.slice().sort(function(){ return Math.random() - 0.5; });
  const exerciciosEscolhidos = embaralhados.slice(0, protocolo.blocos);

  return {
    tabata: true,
    nivel: nivel,
    trabalhoSeg: protocolo.trabalhoSeg,
    descansoSeg: protocolo.descansoSeg,
    ciclos: protocolo.ciclos,
    descricaoProtocolo: protocolo.descricao,
    blocos: exerciciosEscolhidos.map(function(ex){ return { nome: ex.nome }; }),
    duracaoTotalMin: Math.round((protocolo.blocos * protocolo.ciclos * (protocolo.trabalhoSeg + protocolo.descansoSeg)) / 60),
    ex: [] // mantém compatibilidade com qualquer código que espera todo dia ter essa propriedade
  };
}

const mobilidadeBanco = [
  {nome:'Mobilidade Quadril Extensão', trabalho:'Mobilidade /Alongamento', grupo:'Ilio Psoas', video:'https://youtu.be/NrF08RNhKyY'},
  {nome:'Mobilidade Quadril Extensão Diagonal', trabalho:'Mobilidade /Alongamento', grupo:'Ilio Psoas/ Adutores', video:'https://youtu.be/t-xiIUGzO3s'},
  {nome:'Alongamento Adutores Sentado', trabalho:'Alongamento', grupo:'Adutores', video:'https://youtu.be/v0gqJQNR96k'},
  {nome:'Mob.Quadril Adutores 4 Apoios', trabalho:'Mobilidade /Alongamento', grupo:'Adutores', video:'https://youtu.be/eT_hBpaNirQ'},
  {nome:'Alengamento Reto Abdômen', trabalho:'Alongamento', grupo:'Abdomen', video:'https://youtu.be/uw2lrqLXv-I'},
  {nome:'Mob. Quadrado Lombar', trabalho:'Alongamento', grupo:'Quadrado Lombar', video:'https://youtu.be/O2LUiv4LmMU'},
  {nome:'Mob. Glúteo Médio E Máximo', trabalho:'Alongamento', grupo:'Glúteos', video:'https://youtu.be/H_OAHdhD2Ho'},
  {nome:'Mob. Tensor De Fascia Lata', trabalho:'Alongamento', grupo:'Tensor Da Fáscia Lata', video:'https://youtu.be/HAXkyuefy2c'},
  {nome:'Mobilidade Isquiotibiais Em Pé', trabalho:'Alongamento', grupo:'Isquitibiais', video:'https://youtu.be/lmtoMuWbuw4'},
  {nome:'Mob.Deslocamento Lateral Elástico', trabalho:'Mobilidade', grupo:'Glúteo Méd, Mín', video:'https://youtu.be/RWsBO0rqkgU'},
  {nome:'Mob.Retração De Espcapulas', trabalho:'Mobilidade', grupo:'Cintura Escapular', video:'https://youtu.be/d15LLZ_Uxvo'},
  {nome:'Mob.Rotação Externa De Ombro', trabalho:'Mobilidade /Alongamento', grupo:'Cintura Escapular', video:'https://youtu.be/O6sxaHfOR6s'},
  {nome:'Mob. Tornozelo', trabalho:'Mobilidade /Alongamento', grupo:'Tornozelo', video:'https://youtu.be/zgg3NtHWUpc'},
  {nome:'Mob. Quadril Retração', trabalho:'Mobilidade', grupo:'Pelve', video:'https://youtu.be/Yb8-QOlWScM'},
  {nome:'Mob. Ombros Deitado', trabalho:'Mobilidade', grupo:'Cintura Escapular', video:'https://youtu.be/HwDBulMMpcE'},
  {nome:'Along. Isquitibiais Espaldar', trabalho:'Mobilidade', grupo:'Isquitibiais', video:'https://youtu.be/hfKEOGad5dc'},
  {nome:'Along. P Piriforme', trabalho:'Mobilidade', grupo:'Piriforme', video:'https://youtu.be/H_OAHdhD2Ho'},
  {nome:'Along. Peitoral M. Deltoides', trabalho:'Alongamento', grupo:'Peitoral E Deltoide', video:'https://youtu.be/Wh4dRJsVovQ'},
  {nome:'Wall Ball Slide', trabalho:'Mobilidade', grupo:'Serrátil Anterior', video:'https://youtu.be/SBaDnfc9SgE'},
  {nome:'Push Up Plus - Variação', trabalho:'Mobilidade', grupo:'Serrátil Anterior', video:'https://youtu.be/pdVvEmF97Rw'},
  {nome:'Push Up Plus  Com Barra', trabalho:'Mobilidade', grupo:'Serrátil Anterior', video:'https://youtu.be/wlxPLG--5oo'},
  {nome:'Push Up Plus Em 4 Apoios', trabalho:'Mobilidade', grupo:'Serrátil Anterior', video:'https://youtu.be/87IHSrAHdq8'}
];

const aquecimentoBanco = [
  {nome:'Skipping', video:'https://youtu.be/6F0ModFWzIg'},
  {nome:'Polichinelo', video:'https://youtu.be/6AIlJkFNrKA'}
];

const ARTICULACOES = ['Todos', 'Quadril', 'Joelhos', 'Ombros', 'Tornozelos', 'Cintura Escapular', 'Outros'];
let filtroArticulacao = 'Todos';

function categorizarPorArticulacao(grupo){
  const g = (grupo || '').toUpperCase();
  if(g.indexOf('ILIO PSOAS') !== -1 || g.indexOf('ADUTORES') !== -1 || g.indexOf('PELVE') !== -1 || g.indexOf('PIRIFORME') !== -1 || g.indexOf('GLÚTEO') !== -1 || g.indexOf('FÁSCIA LATA') !== -1 || g.indexOf('QUADRADO LOMBAR') !== -1) return 'Quadril';
  if(g.indexOf('QUADRÍCEPS') !== -1 || g.indexOf('ISQUI') !== -1) return 'Joelhos';
  if(g.indexOf('OMBRO') !== -1 || g.indexOf('PEITORAL') !== -1 || g.indexOf('PEITO') !== -1 || g.indexOf('SERRÁTIL') !== -1) return 'Ombros';
  if(g.indexOf('TORNOZELO') !== -1 || g.indexOf('PANTURRILHA') !== -1) return 'Tornozelos';
  if(g.indexOf('CINTURA ESCAPULAR') !== -1 || g.indexOf('COSTAS') !== -1) return 'Cintura Escapular';
  return 'Outros';
}

function renderArticulacaoChips(){
  const container = document.getElementById('mob-articulacao-lista');
  if(!container) return;
  container.innerHTML = '';
  document.getElementById('mob-count-label').textContent = mobilidadeBanco.length + ' itens cadastrados';
  ARTICULACOES.filter(function(a){ return a !== 'Todos'; }).forEach(function(art){
    const qtd = mobilidadeBanco.filter(function(m){ return categorizarPorArticulacao(m.grupo) === art; }).length;
    const btn = document.createElement('div');
    btn.className = 'list-item';
    btn.style.cursor = 'pointer';
    btn.innerHTML = '<span>' + art + '</span><span class="tag">' + qtd + '</span>';
    btn.onclick = function(){ abrirArticulacao(art); };
    container.appendChild(btn);
  });
}

function abrirArticulacao(art){
  filtroArticulacao = art;
  document.getElementById('mob-articulacoes-view').style.display = 'none';
  document.getElementById('mob-lista-view').style.display = 'block';
  document.getElementById('mob-articulacao-titulo').textContent = art;
  renderMobilidadeBanco();
}

function voltarParaArticulacoes(){
  document.getElementById('mob-lista-view').style.display = 'none';
  document.getElementById('mob-articulacoes-view').style.display = 'block';
  renderArticulacaoChips();
  renderAquecimentoBanco();
}

function renderMobilidadeBanco(){
  const list = document.getElementById('mobilidade-list-personal');
  const termo = (document.getElementById('mob-search').value || '').toUpperCase();
  list.innerHTML = '';
  const filtrados = mobilidadeBanco.filter(function(m){
    const bateArticulacao = filtroArticulacao === 'Todos' || categorizarPorArticulacao(m.grupo) === filtroArticulacao;
    const bateTermo = !termo || m.nome.toUpperCase().indexOf(termo) !== -1 || m.grupo.toUpperCase().indexOf(termo) !== -1;
    return bateArticulacao && bateTermo;
  });
  filtrados.forEach(function(m){
    const el = document.createElement('div');
    el.className = 'exercicio-item';
    el.innerHTML =
      '<p class="ex-name">' + m.nome + '</p>' +
      '<p class="ex-meta">' + m.trabalho + ' · ' + m.grupo + '</p>' +
      '<a class="ex-video ok" href="' + m.video + '" target="_blank" rel="noopener"><i class="ti ti-circle-check" style="font-size:11px;vertical-align:-1px;margin-right:3px;"></i>Ver vídeo</a>';
    list.appendChild(el);
  });
}

function renderAquecimentoBanco(){
  const list = document.getElementById('aquecimento-list-personal');
  list.innerHTML = '';
  aquecimentoBanco.forEach(function(a){
    const el = document.createElement('div');
    el.className = 'exercicio-item';
    el.innerHTML =
      '<p class="ex-name">' + a.nome + '</p>' +
      '<a class="ex-video ok" href="' + a.video + '" target="_blank" rel="noopener"><i class="ti ti-circle-check" style="font-size:11px;vertical-align:-1px;margin-right:3px;"></i>Ver vídeo</a>';
    list.appendChild(el);
  });
}

function populateGrupoSelect(){
  const sel = document.getElementById('ex-grupo');
  if(sel.options.length > 0) return;
  categoriasMusculo.forEach(function(cat){
    const opt = document.createElement('option');
    opt.value = cat;
    opt.textContent = cat;
    sel.appendChild(opt);
  });
}

function alternarSubabaExercicios(qual){
  document.getElementById('subaba-videos-btn').className = 'chip' + (qual === 'videos' ? ' active' : '');
  document.getElementById('subaba-metodos-btn').className = 'chip' + (qual === 'metodos' ? ' active' : '');
  document.getElementById('subaba-videos-container').style.display = qual === 'videos' ? 'block' : 'none';
  document.getElementById('subaba-metodos-view').style.display = qual === 'metodos' ? 'block' : 'none';
  if(qual === 'metodos') renderMetodosReferencia();
}

function renderMetodosReferencia(){
  const view = document.getElementById('subaba-metodos-view');
  const metodos = conteudos.filter(function(c){ return c.cat === 'Métodos de treino'; });
  let html = '<p class="page-sub" style="margin-top:-4px;">Referência dos métodos de intensidade, mesmo conteúdo já disponível na Mentoria</p>';
  metodos.forEach(function(m){
    html += '<div class="exercicio-item" style="cursor:pointer;" onclick="playConteudoMetodo(\'' + m.n.replace(/'/g,"\\'") + '\')">' +
      '<p class="ex-name">' + m.n + '</p>' +
      '<p class="ex-meta">' + m.desc + '</p>' +
      (m.video ? '<span class="ex-video ok"><i class="ti ti-circle-check" style="font-size:11px;vertical-align:-1px;margin-right:3px;"></i>Assistir</span>' : '<span class="ex-video pending"><i class="ti ti-clock" style="font-size:11px;vertical-align:-1px;margin-right:3px;"></i>Sem vídeo ainda</span>') +
      '</div>';
  });
  view.innerHTML = html;
}

function playConteudoMetodo(nome){
  const c = conteudos.find(function(x){ return x.n === nome; });
  if(!c) return;
  const embed = getEmbedUrl(c.video);
  const view = document.getElementById('subaba-metodos-view');
  view.innerHTML = '<div class="local-back" onclick="renderMetodosReferencia()"><i class="ti ti-arrow-left"></i><span>Voltar</span></div>' +
    (embed ? '<div class="video-block" style="margin-top:14px;"><iframe src="' + embed + '" title="' + c.n + '" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></div>' + ytFallback(c.video) : '<div class="video-block" style="margin-top:14px;"><i class="ti ti-player-play"></i></div>') +
    '<h1 class="page-title" style="margin-top:10px;">' + c.n + '</h1>' +
    '<p class="page-sub">' + c.desc + '</p>';
}

async function excluirExercicioBanco(nome){
  if(!confirm('Excluir "' + nome + '" do banco de exercícios? Isso não pode ser desfeito, e vale pra sempre (mesmo depois de atualizar a página).')) return;
  const idx = exerciciosBanco.findIndex(function(e){ return e.nome === nome; });
  if(idx !== -1) exerciciosBanco.splice(idx, 1);
  renderExerciciosLista();
  await salvarCatalogoPersonal('exercicio_excluido', nome, { nome: nome });
}

async function salvarCatalogoPersonal(tipo, chave, dados){
  if(!supabaseClient) return;
  try {
    await supabaseClient.from('catalogo_personal').upsert({
      tipo: tipo,
      chave: chave,
      dados: dados,
      updated_at: new Date().toISOString()
    });
  } catch(erroDeRede){
    console.warn('Sem conexão agora, isso ficou salvo só localmente por enquanto:', erroDeRede);
  }
}

let catalogoPersonalJaCarregado = false;
async function carregarCatalogoPersonal(){
  if(!supabaseClient || catalogoPersonalJaCarregado) return;
  try {
    const { data: linhas } = await supabaseClient.from('catalogo_personal').select('*');
    if(!linhas) return;
    catalogoPersonalJaCarregado = true;

    linhas.forEach(function(row){
      if(row.tipo === 'exercicio_customizado'){
        const jaExiste = exerciciosBanco.find(function(e){ return e.nome === row.dados.nome; });
        if(!jaExiste) exerciciosBanco.push(row.dados);
      } else if(row.tipo === 'exercicio_movido'){
        const ex = exerciciosBanco.find(function(e){ return e.nome === row.dados.nome; });
        if(ex){ ex.categoria = row.dados.categoria; ex.grupo = row.dados.categoria; }
      } else if(row.tipo === 'exercicio_editado'){
        const ex = exerciciosBanco.find(function(e){ return e.nome === row.dados.nomeAntigo; });
        if(ex){ ex.nome = row.dados.nome; ex.video = row.dados.video; }
      } else if(row.tipo === 'conteudo'){
        const jaExisteConteudo = conteudos.find(function(c){ return c.n === row.dados.n; });
        if(!jaExisteConteudo){
          conteudos.push(row.dados);
        } else {
          jaExisteConteudo.exclusivoDeCurso = row.dados.exclusivoDeCurso;
        }
      } else if(row.tipo === 'curso'){
        const jaExisteCurso = cursosPersonal.find(function(c){ return c.id === row.dados.id; });
        if(!jaExisteCurso) cursosPersonal.push(row.dados);
      } else if(row.tipo === 'feedback_gerador'){
        const listaLocal = carregarFeedbackGerador();
        const jaTem = listaLocal.some(function(f){ return f.data === row.dados.data && f.aluna === row.dados.aluna && f.texto === row.dados.texto; });
        if(!jaTem){ listaLocal.push(row.dados); salvarListaFeedbackGerador(listaLocal); }
      } else if(row.tipo === 'config_ranking'){
        if(row.dados.meta) metaComunidadePontos = row.dados.meta;
        if(row.dados.recompensa) metaComunidadeRecompensa = row.dados.recompensa;
      } else if(row.tipo === 'config_meta_financeira'){
        if(row.dados.meta != null) metaFaturamentoMensal = row.dados.meta;
      } else if(row.tipo === 'relatorio_tendencias'){
        const jaExisteRelatorio = relatoriosTendencias.find(function(r){ return r.data === row.dados.data; });
        if(!jaExisteRelatorio) relatoriosTendencias.push(row.dados);
      }
    });

    // Processa exclusões por último, pra garantir que remove mesmo que algo tenha tentado re-adicionar antes
    linhas.forEach(function(row){
      if(row.tipo === 'exercicio_excluido'){
        const idx = exerciciosBanco.findIndex(function(e){ return e.nome === row.dados.nome; });
        if(idx !== -1) exerciciosBanco.splice(idx, 1);
      }
    });

    renderExerciciosChips();
    if(document.getElementById('cursos-list-personal')) renderCursosPersonal();
    if(document.getElementById('conteudo-list-personal')) renderConteudoPersonal();
  } catch(erroDeRede){
    console.warn('Sem conexão pra carregar o catálogo salvo agora:', erroDeRede);
  }
}

function addExercicio(){
  const nome = document.getElementById('ex-nome').value.trim();
  if(!nome) return;
  const cat = document.getElementById('ex-grupo').value;
  const novoExercicio = {
    nome: nome,
    grupo: cat,
    categoria: cat,
    ambiente: 'Academia',
    nivel: document.getElementById('ex-nivel').value,
    metodo: document.getElementById('ex-metodo').value,
    video: document.getElementById('ex-video').value.trim()
  };
  exerciciosBanco.push(novoExercicio);
  salvarCatalogoPersonal('exercicio_customizado', nome, novoExercicio);
  document.getElementById('ex-nome').value = '';
  document.getElementById('ex-video').value = '';
  mostrandoFormAdicionar = false;
  filtroExercicio = cat;
  renderExerciciosChips();
  renderExerciciosLista();
}

let cursosPersonal = [];
let cursoEmEdicaoId = null;
let relatoriosTendencias = [];

// ===== RELATÓRIO DE TENDÊNCIAS DO NICHO (inteligência de mercado) =====
async function gerarRelatorioTendencias(){
  const area = document.getElementById('relatorio-tendencias-area');
  if(!area) return;
  area.innerHTML = '<div class="info-box"><p class="txt" style="color:var(--text-faint);">Pesquisando o que está em alta no nicho agora...</p></div>';

  const temasJaCobertos = relatoriosTendencias.slice(-5).map(function(r){ return r.temasPrincipais || ''; }).filter(Boolean).join(' | ');

  try {
    const response = await fetch(SUPABASE_URL + '/functions/v1/pesquisar-tendencias', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json', 'apikey': SUPABASE_ANON_KEY, 'Authorization': 'Bearer ' + SUPABASE_ANON_KEY },
      body: JSON.stringify({ temasJaCobertos: temasJaCobertos })
    });
    const data = await response.json();
    if(!response.ok || data.error){
      throw new Error('HTTP ' + response.status + ': ' + (data.error || JSON.stringify(data)));
    }
    // A própria Edge Function já salva no Supabase (funciona também quando chamada pelo cron, sem navegador aberto).
    // Aqui só guardamos localmente pra atualizar a tela na hora, usando a mesma data que o servidor usou pra salvar.
    const registro = { data: data.data || new Date().toISOString(), texto: data.relatorio || 'Não consegui gerar o relatório agora.', temasPrincipais: data.temasPrincipais || '' };
    relatoriosTendencias.push(registro);
    renderRelatoriosTendencias();
  } catch(erro){
    console.error('[Relatório de tendências] erro real:', erro);
    area.innerHTML = '<div class="info-box"><p class="txt" style="color:#E2A33D;">Não consegui gerar o relatório agora. Tenta de novo em instantes.</p></div>';
  }
}

function renderRelatoriosTendencias(){
  const area = document.getElementById('relatorio-tendencias-area');
  if(!area) return;
  if(relatoriosTendencias.length === 0){ area.innerHTML = '<p class="txt" style="color:var(--text-faint);">Nenhum relatório gerado ainda.</p>'; return; }
  const ordenados = relatoriosTendencias.slice().sort(function(a,b){ return new Date(b.data) - new Date(a.data); });
  area.innerHTML = ordenados.map(function(r, i){
    const dataFormatada = new Date(r.data).toLocaleDateString('pt-BR', { day:'2-digit', month:'2-digit', year:'numeric', hour:'2-digit', minute:'2-digit' });
    return '<div class="info-box" style="margin-bottom:10px;">' +
      '<p class="lbl" style="margin-bottom:6px;">' + dataFormatada + (i === 0 ? ' <span class="tag" style="background:var(--success-soft);color:var(--success);">mais recente</span>' : '') + '</p>' +
      '<p class="txt" style="white-space:pre-wrap;">' + r.texto + '</p>' +
    '</div>';
  }).join('');
}

function renderConteudoPersonal(){
  const list = document.getElementById('conteudo-list-personal');
  list.innerHTML = '';
  conteudos.forEach(function(c, idx){
    const el = document.createElement('div');
    el.className = 'exercicio-item';
    el.innerHTML =
      '<p class="ex-name">' + c.n + (c.locked ? ' <i class="ti ti-lock" style="font-size:11px;color:var(--gold-soft);"></i>' : '') + '</p>' +
      '<p class="ex-meta">' + c.cat + '</p>' +
      '<label style="display:flex;align-items:center;gap:6px;font-size:10.5px;color:var(--text-faint);margin-top:4px;">' +
        '<input type="checkbox" style="width:13px;height:13px;" ' + (c.exclusivoDeCurso ? 'checked' : '') + ' onchange="alternarExclusividadeItemCurso(' + idx + ')"> Exclusivo de curso (não aparece solto na Mentoria)' +
      '</label>';
    list.appendChild(el);
  });
}
renderConteudoPersonal();
renderCursosPersonal();

function gerarIdCurso(){ return 'curso_' + Date.now() + '_' + Math.floor(Math.random()*1000); }

function mostrarFormularioCurso(idCurso){
  cursoEmEdicaoId = idCurso || null;
  const curso = idCurso ? cursosPersonal.find(function(c){ return c.id === idCurso; }) : null;
  const area = document.getElementById('form-curso-area');

  area.innerHTML = '<div class="info-box" style="margin-bottom:14px;">' +
    '<p class="lbl" style="margin-bottom:10px;">' + (curso ? 'Editar curso' : 'Novo curso') + '</p>' +
    '<div class="form-group"><label class="form-label">Nome do curso</label><input class="form-input" id="curso-nome" value="' + (curso ? curso.nome.replace(/"/g,'&quot;') : '') + '" placeholder="Ex: Corpo de Musa"></div>' +
    '<div class="form-group"><label class="form-label">Valor (opcional, deixe em branco se for gratuito)</label><input class="form-input" id="curso-preco" value="' + (curso && curso.preco ? curso.preco : '') + '" placeholder="Ex: R$ 97,00"></div>' +
    '<div class="form-group" style="display:flex;align-items:center;gap:8px;">' +
      '<input type="checkbox" id="curso-aberto" style="width:16px;height:16px;" ' + (curso && curso.aberto ? 'checked' : '') + '>' +
      '<label class="form-label" style="margin:0;" for="curso-aberto">Aberto pra todas as alunas (sem precisar liberar uma por uma)</label>' +
    '</div>' +
    '<p class="form-label" style="margin-top:10px;">Conteúdo desse curso</p>' +
    '<div id="curso-itens-picker" style="max-height:180px;overflow-y:auto;display:flex;flex-direction:column;gap:4px;margin-bottom:10px;"></div>' +
    (curso && !curso.aberto ? '<p class="form-label" style="margin-top:10px;">Liberar manualmente pra alunas específicas</p><div id="curso-alunas-picker" style="max-height:180px;overflow-y:auto;display:flex;flex-direction:column;gap:4px;margin-bottom:10px;"></div>' : '<div id="curso-alunas-picker"></div>') +
    '<div style="display:flex;gap:8px;margin-top:10px;">' +
      '<button class="btn-gold" onclick="salvarCurso()">' + (curso ? 'Salvar alterações' : 'Criar curso') + '</button>' +
      '<button class="btn-gold" style="background:var(--card-2);color:var(--text-faint);border:1px solid var(--border);" onclick="document.getElementById(\'form-curso-area\').innerHTML=\'\';">Cancelar</button>' +
    '</div>' +
  '</div>';

  const pickerItens = document.getElementById('curso-itens-picker');
  const itensSelecionados = curso ? curso.itensIds || [] : [];
  pickerItens.innerHTML = conteudos.map(function(c, idx){
    const marcado = itensSelecionados.indexOf(idx) !== -1;
    return '<label style="display:flex;align-items:center;gap:8px;font-size:12px;"><input type="checkbox" class="curso-item-check" value="' + idx + '" ' + (marcado ? 'checked' : '') + '> ' + c.n + '</label>';
  }).join('');

  const pickerAlunas = document.getElementById('curso-alunas-picker');
  if(pickerAlunas){
    const alunasLiberadas = curso ? curso.alunasLiberadas || [] : [];
    pickerAlunas.innerHTML = alunasPersonal.slice(0, 100).map(function(a){
      const marcado = alunasLiberadas.indexOf(a.nome) !== -1;
      return '<label style="display:flex;align-items:center;gap:8px;font-size:12px;"><input type="checkbox" class="curso-aluna-check" value="' + a.nome.replace(/"/g,'&quot;') + '" ' + (marcado ? 'checked' : '') + '> ' + a.nome + '</label>';
    }).join('') + (alunasPersonal.length > 100 ? '<p class="txt" style="font-size:11px;color:var(--text-faint);">Mostrando as 100 primeiras — usa a busca de alunas fora daqui pra liberar outras.</p>' : '');
  }
}

function salvarCurso(){
  const nome = document.getElementById('curso-nome').value.trim();
  if(!nome){ alert('Dá um nome pro curso primeiro.'); return; }
  const preco = document.getElementById('curso-preco').value.trim();
  const aberto = document.getElementById('curso-aberto').checked;
  const itensIds = Array.from(document.querySelectorAll('.curso-item-check:checked')).map(function(el){ return parseInt(el.value, 10); });
  const alunasLiberadas = Array.from(document.querySelectorAll('.curso-aluna-check:checked')).map(function(el){ return el.value; });

  let curso = cursoEmEdicaoId ? cursosPersonal.find(function(c){ return c.id === cursoEmEdicaoId; }) : null;
  if(!curso){
    curso = { id: gerarIdCurso() };
    cursosPersonal.push(curso);
  }
  curso.nome = nome;
  curso.preco = preco;
  curso.aberto = aberto;
  curso.itensIds = itensIds;
  curso.alunasLiberadas = alunasLiberadas;

  salvarCatalogoPersonal('curso', curso.id, curso);
  document.getElementById('form-curso-area').innerHTML = '';
  cursoEmEdicaoId = null;
  renderCursosPersonal();
}

function alternarExclusividadeItemCurso(idxConteudo){
  const c = conteudos[idxConteudo];
  if(!c) return;
  c.exclusivoDeCurso = !c.exclusivoDeCurso;
  salvarCatalogoPersonal('conteudo', c.n, c);
  renderCursosPersonal();
}

function renderCursosPersonal(){
  const container = document.getElementById('cursos-list-personal');
  if(!container) return;
  if(cursosPersonal.length === 0){
    container.innerHTML = '';
    return;
  }
  container.innerHTML = cursosPersonal.map(function(curso){
    const qtdItens = (curso.itensIds || []).length;
    const qtdAlunas = (curso.alunasLiberadas || []).length;
    return '<div class="info-box" style="margin-bottom:10px;">' +
      '<p class="lbl">' + curso.nome + (curso.preco ? ' · ' + curso.preco : ' · Gratuito') + '</p>' +
      '<p class="txt" style="font-size:11px;color:var(--text-faint);">' + qtdItens + ' conteúdo(s) · ' + (curso.aberto ? 'Aberto pra todas' : qtdAlunas + ' aluna(s) liberada(s)') + '</p>' +
      '<div style="display:flex;gap:8px;margin-top:8px;">' +
        '<span class="chip" style="cursor:pointer;" onclick="mostrarFormularioCurso(\'' + curso.id + '\')">Editar</span>' +
      '</div>' +
    '</div>';
  }).join('');
}

function addConteudo(){
  const titulo = document.getElementById('ct-titulo').value.trim();
  if(!titulo) return;
  const novoConteudo = {
    n: titulo,
    cat: document.getElementById('ct-categoria').value,
    locked: document.getElementById('ct-locked').checked,
    desc: document.getElementById('ct-desc').value.trim() || 'Sem descrição ainda.'
  };
  conteudos.push(novoConteudo);
  salvarCatalogoPersonal('conteudo', titulo, novoConteudo);
  document.getElementById('ct-titulo').value = '';
  document.getElementById('ct-desc').value = '';
  document.getElementById('ct-video').value = '';
  document.getElementById('ct-locked').checked = false;
  renderConteudoPersonal();
  renderGrid();
}

/* ===== BIBLIOTECA DE TREINOS ===== */

const volumeRegra = {
  'Iniciante': 'Até 20 séries',
  'Intermediário': '25 a 30 séries',
  'Avançado': 'Até 50 séries'
};

const templatesBiblioteca = [
  {nivel:'Iniciante', enfase:'Emagrecimento', freq:'3x', volume:volumeRegra['Iniciante'], exercicios:['Agachamento livre', 'Leg press']},
  {nivel:'Intermediário', enfase:'Glúteo', freq:'5x', volume:volumeRegra['Intermediário'], exercicios:['Elevação pélvica', 'Leg press', 'Cadeira extensora']},
  {nivel:'Avançado', enfase:'Glúteo', freq:'6x', volume:volumeRegra['Avançado'], exercicios:['Agachamento livre', 'Leg press', 'Elevação pélvica', 'Cadeira extensora']}
];

let templateExerciciosTemp = [];

function renderTemplates(){
  const nivel = document.getElementById('tp-filtro-nivel').value;
  const enfase = document.getElementById('tp-filtro-enfase').value;
  const freq = document.getElementById('tp-filtro-freq').value;
  const list = document.getElementById('templates-list');
  list.innerHTML = '';
  const filtrados = templatesBiblioteca.filter(function(t){
    return (nivel === 'Todos' || t.nivel === nivel) &&
           (enfase === 'Todas' || t.enfase === enfase) &&
           (freq === 'Todas' || t.freq === freq);
  });
  if(filtrados.length === 0){
    list.innerHTML = '<div class="info-box"><p class="txt">Nenhum template com esse filtro ainda.</p></div>';
    return;
  }
  filtrados.forEach(function(t){
    const el = document.createElement('div');
    el.className = 'info-box';
    el.innerHTML =
      '<p class="lbl">' + t.nivel + ' · ' + t.enfase + ' · ' + t.freq + '</p>' +
      '<p class="txt">Volume: ' + t.volume + '</p>' +
      '<p class="txt">Exercícios: ' + t.exercicios.join(', ') + '</p>' +
      '<p class="txt" style="color:var(--text-faint);font-size:12px;">Progressão: teste de 10RM subjetivo por sessão · exercícios evoluem de complexidade básica para avançada conforme o nível da aluna.</p>';
    list.appendChild(el);
  });
}

function updateVolumeHint(){
  const nivel = document.getElementById('tp-nivel').value;
  document.getElementById('tp-volume-hint').innerHTML = '<span>' + volumeRegra[nivel] + '</span>';
}

function populateExercicioSelect(){
  const sel = document.getElementById('tp-exercicio-select');
  sel.innerHTML = '';
  exerciciosBanco.forEach(function(e){
    const opt = document.createElement('option');
    opt.value = e.nome;
    opt.textContent = e.nome;
    sel.appendChild(opt);
  });
  updateVolumeHint();
}

function addExercicioAoTemplate(){
  const nome = document.getElementById('tp-exercicio-select').value;
  if(!nome || templateExerciciosTemp.indexOf(nome) !== -1) return;
  templateExerciciosTemp.push(nome);
  renderExerciciosSelecionados();
}

function renderExerciciosSelecionados(){
  const wrap = document.getElementById('tp-exercicios-selecionados');
  wrap.innerHTML = '';
  templateExerciciosTemp.forEach(function(nome){
    const chip = document.createElement('span');
    chip.className = 'desvio-chip selected';
    chip.textContent = nome + ' ×';
    chip.onclick = function(){
      templateExerciciosTemp = templateExerciciosTemp.filter(function(n){ return n !== nome; });
      renderExerciciosSelecionados();
    };
    wrap.appendChild(chip);
  });
}

function salvarTemplate(){
  const nivel = document.getElementById('tp-nivel').value;
  const enfase = document.getElementById('tp-enfase').value.trim();
  const freq = document.getElementById('tp-freq').value;
  if(!enfase || templateExerciciosTemp.length === 0) return;
  templatesBiblioteca.push({
    nivel: nivel, enfase: enfase, freq: freq,
    volume: volumeRegra[nivel],
    exercicios: templateExerciciosTemp.slice()
  });
  document.getElementById('tp-enfase').value = '';
  templateExerciciosTemp = [];
  renderExerciciosSelecionados();
  renderTemplates();
}

/* ===== GRUPO DE DESAFIO ===== */

const desafios = [
  {nome:'Desafio 7 Dias Glúteo de Aço', treino:'Intermediário · Glúteo · 5x', inscritas:34, link:'musa.plus/desafio/gluteo-de-aco'}
];

function renderDesafios(){
  const list = document.getElementById('desafios-list');
  list.innerHTML = '';
  desafios.forEach(function(d){
    const el = document.createElement('div');
    el.className = 'info-box';
    el.innerHTML =
      '<p class="lbl">' + d.nome + '</p>' +
      '<p class="txt">Treino: ' + d.treino + '</p>' +
      '<p class="txt">' + d.inscritas + ' inscritas</p>' +
      '<div class="list-item" style="margin-top:6px;"><span style="color:var(--gold-soft);">' + d.link + '</span><i class="ti ti-copy" style="font-size:13px;color:var(--text-dim);"></i></div>';
    list.appendChild(el);
  });
}

function populateTreinoSelect(){
  const sel = document.getElementById('df-treino');
  sel.innerHTML = '';
  templatesBiblioteca.forEach(function(t){
    const opt = document.createElement('option');
    const label = t.nivel + ' · ' + t.enfase + ' · ' + t.freq;
    opt.value = label;
    opt.textContent = label;
    sel.appendChild(opt);
  });
}

function criarDesafio(){
  const nome = document.getElementById('df-nome').value.trim();
  if(!nome) return;
  const slug = nome.toLowerCase().normalize('NFD').replace(/[\u0300-\u036f]/g,'').replace(/[^a-z0-9]+/g,'-').replace(/(^-|-$)/g,'');
  desafios.push({
    nome: nome,
    treino: document.getElementById('df-treino').value,
    inscritas: 0,
    link: 'musa.plus/desafio/' + slug
  });
  document.getElementById('df-nome').value = '';
  document.getElementById('df-duracao').value = '';
  renderDesafios();
}

function aplicarTransicaoSuave(elId){
  const el = document.getElementById(elId);
  if(!el) return;
  el.classList.remove('fade-content');
  void el.offsetWidth;
  el.classList.add('fade-content');
}

/* ===== LOGIN ===== */
const EMAIL_PERSONAL = 'thiagofernandesdearaujo22@gmail.com';
const SENHA_PERSONAL = 'Senhanova22-'; // ⚠️ exposta em texto puro nesse protótipo, troque antes de liberar pra alunas reais

function alternarTipoLogin(tipo){
  document.getElementById('login-tab-aluna').className = 'chip' + (tipo === 'aluna' ? ' active' : '');
  document.getElementById('login-tab-personal').className = 'chip' + (tipo === 'personal' ? ' active' : '');
  document.getElementById('login-form-aluna').style.display = tipo === 'aluna' ? 'block' : 'none';
  document.getElementById('login-form-personal').style.display = tipo === 'personal' ? 'block' : 'none';
}

async function loginAluna(){
  const email = document.getElementById('login-aluna-email').value.trim().toLowerCase();
  const senha = document.getElementById('login-aluna-senha').value;
  const erroEl = document.getElementById('login-aluna-erro');
  erroEl.style.display = 'none';

  if(!email || !senha){
    erroEl.textContent = 'Preencha e-mail e senha.';
    erroEl.style.display = 'block';
    return;
  }

  if(!supabaseClient){
    entrarLocalPorEmail(email);
    return;
  }

  try {
    let primeiroAcesso = false;
    let { data, error } = await supabaseClient.auth.signInWithPassword({ email: email, password: senha });

    if(error){
      // E-mail e senha corretos, mas a conta ainda não foi confirmada (ajuste isso no painel do Supabase: Authentication → Providers → Email → desative "Confirm email")
      if(error.message.toLowerCase().indexOf('email not confirmed') !== -1){
        erroEl.textContent = 'Sua conta ainda não foi liberada. Peça pro seu personal confirmar seu acesso.';
        erroEl.style.display = 'block';
        return;
      }
      // Senha errada numa conta que já existe (não confundir com primeiro acesso)
      if(error.message.toLowerCase().indexOf('invalid login credentials') !== -1){
        const jaTemConta = await supabaseClient.from('alunas').select('auth_id').eq('email', email).maybeSingle();
        if(jaTemConta.data && jaTemConta.data.auth_id){
          erroEl.textContent = 'Senha incorreta.';
          erroEl.style.display = 'block';
          return;
        }
      }
      // Ainda não existe login com esse e-mail: cadastra agora (primeiro acesso, mesmo que já tenha dados da anamnese)
      const cadastro = await supabaseClient.auth.signUp({ email: email, password: senha });
      if(cadastro.error){
        erroEl.textContent = cadastro.error.message.indexOf('Password') !== -1 ? 'Senha precisa ter pelo menos 6 caracteres.' : cadastro.error.message;
        erroEl.style.display = 'block';
        return;
      }
      data = cadastro.data;
      primeiroAcesso = true;
      const nomeNovaAluna = email.split('@')[0];
      await supabaseClient.from('perfis').insert({ id: data.user.id, tipo: 'aluna', nome: nomeNovaAluna });
      await supabaseClient.from('alunas').upsert({ email: email, auth_id: data.user.id, nome: nomeNovaAluna }, { onConflict: 'email' });
      // Garante que ela também exista localmente, pra todas as telas já funcionarem
      alunasPersonal.push({ nome: nomeNovaAluna, status: 'ok', statusLabel: 'Ativa recente', nivel: 'Iniciante', freq: '3x por semana', email: email, telefone: '', piramide: '', objetivo: '', restricoes: '', academia: '', dataAnamnese: new Date().toISOString().slice(0,10) });
    }

    sessaoUsuarioAtual = data.user;

    // Descobre o nome real dela (pode já existir cadastro feito pelo Personal com o mesmo e-mail)
    const { data: alunaRow, error: erroAlunaRow } = await supabaseClient.from('alunas').select('*').eq('auth_id', data.user.id).maybeSingle();

    if(erroAlunaRow || !alunaRow){
      // NUNCA deixa passar em silêncio — sem isso, o sistema ficaria com o nome padrão travado e mostraria dados de outra pessoa
      erroEl.textContent = 'Login funcionou, mas não achei seu cadastro completo no banco. Avisa seu personal (erro: cadastro sem auth_id vinculado).';
      erroEl.style.display = 'block';
      return;
    }
    NOME_ALUNA_LOGADA = alunaRow.nome;

    // Se ela ainda não existe na lista local dessa sessão (primeiro login no aparelho dela), cria o registro completo agora,
    // com os dados reais dela — sem isso, os cálculos de DNA/progresso ficariam usando dados de outra aluna que sobraram por padrão
    let alunaLocal = alunasPersonal.find(function(x){ return x.nome === NOME_ALUNA_LOGADA; });
    if(!alunaLocal && alunaRow){
      alunaLocal = {
        nome: alunaRow.nome,
        email: alunaRow.email,
        telefone: alunaRow.telefone || '',
        nivel: alunaRow.nivel || 'Iniciante',
        freq: alunaRow.freq || '3x por semana',
        piramide: alunaRow.piramide || '',
        objetivo: alunaRow.objetivo || '',
        restricoes: alunaRow.restricoes || 'Nenhuma relatada',
        academia: alunaRow.academia || '',
        idade: alunaRow.idade || null,
        dataAnamnese: alunaRow.data_anamnese || new Date().toISOString().slice(0,10),
        status: 'ok',
        statusLabel: 'Ativa recente'
      };
      if(alunaRow.dados_extras && typeof alunaRow.dados_extras === 'object'){
        Object.keys(alunaRow.dados_extras).forEach(function(campo){
          if(alunaRow.dados_extras[campo] != null) alunaLocal[campo] = alunaRow.dados_extras[campo];
        });
      }
      alunasPersonal.push(alunaLocal);
    }
    if(alunaLocal && alunaLocal.treinoAtual && alunaLocal.treinoAtual.dias){
      dias = alunaLocal.treinoAtual.dias;
    }
    aplicarIdentidadeVisual(NOME_ALUNA_LOGADA);
    pedirPermissaoNotificacao();
    verificarPerguntarSobreAvisos();

    sessaoTipo = 'aluna';
    document.getElementById('card-personal-launcher').style.display = 'none';

    await carregarELigarTreinoDaAluna(data.user.id);
    await carregarProgressoDoSupabase(data.user.id, NOME_ALUNA_LOGADA);
    renderBadgeAvisos(); // só depois do progresso carregado, senão mostra 0 avisos por engano

    const dentroDoLimiteDeIntro = deveExibirEEIncrementarIntro(NOME_ALUNA_LOGADA);
    if(primeiroAcesso){
      setActive('intro');
      timerIntroAtual = setTimeout(function(){ setActive('onboarding'); }, 4000);
    } else if(dentroDoLimiteDeIntro){
      setActive('intro');
      timerIntroAtual = setTimeout(function(){ setActive('launcher'); }, 4000);
    } else {
      setActive('launcher');
    }
    perguntarSeQuerLembrarLogin();
  } catch(erroDeRede){
    // Falha de rede de verdade (ex: "Failed to fetch") — nunca trava o teste aqui, entra local
    console.warn('Sem conexão com o Supabase agora, entrando localmente pra você continuar testando:', erroDeRede);
    entrarLocalPorEmail(email);
  }
}

function entrarLocalPorEmail(email){
  if(supabaseClient) supabaseClient.auth.signOut().catch(function(){});
  const encontrada = alunasPersonal.find(function(a){ return (a.email || '').toLowerCase() === email; });
  NOME_ALUNA_LOGADA = encontrada ? encontrada.nome : NOME_ALUNA_LOGADA;
  if(encontrada && encontrada.treinoAtual && encontrada.treinoAtual.dias){
    dias = encontrada.treinoAtual.dias;
  }
  aplicarIdentidadeVisual(NOME_ALUNA_LOGADA);
  pedirPermissaoNotificacao();
  verificarPerguntarSobreAvisos();
  renderBadgeAvisos();
  sessaoTipo = 'aluna';
  document.getElementById('card-personal-launcher').style.display = 'none';
  setActive('launcher');
}

async function carregarELigarTreinoDaAluna(userId){
  try {
    const { data: treinoData } = await supabaseClient
      .from('treinos')
      .select('*')
      .eq('aluna_id', userId)
      .order('updated_at', { ascending: false })
      .limit(1)
      .maybeSingle();

    if(treinoData){
      aplicarTreinoRecebidoDoSupabase(treinoData);
    } else {
      // Ainda não tem treino no lugar "oficial" — pode ter sido feito antes dela ter login, verifica o backup
      const { data: alunaRow } = await supabaseClient.from('alunas').select('treino_atual_backup').eq('auth_id', userId).maybeSingle();
      if(alunaRow && alunaRow.treino_atual_backup){
        aplicarTreinoRecebidoDoSupabase({ fase: alunaRow.treino_atual_backup.fase, volume: alunaRow.treino_atual_backup.volume, dias: alunaRow.treino_atual_backup.dias });
      }
    }

    const canalRealtime = supabaseClient
      .channel('treino-da-aluna-' + userId)
      .on('postgres_changes', { event: '*', schema: 'public', table: 'treinos', filter: 'aluna_id=eq.' + userId }, function(payload){
        console.log('[Tempo real] Evento recebido:', payload);
        const aviso = document.createElement('div');
        aviso.style.cssText = 'position:fixed;top:8px;left:8px;right:8px;background:#1a3a1a;color:#8FAE7D;padding:10px;border-radius:10px;font-size:11px;text-align:center;z-index:99999;';
        aviso.textContent = '[Diagnóstico] Treino atualizado em tempo real, chegou agora.';
        document.body.appendChild(aviso);
        setTimeout(function(){ aviso.remove(); }, 6000);
        if(payload.new) aplicarTreinoRecebidoDoSupabase(payload.new);
      })
      .subscribe(function(status){
        console.log('[Tempo real] Status da inscrição:', status);
        const aviso = document.createElement('div');
        aviso.id = 'diagnostico-realtime-status';
        aviso.style.cssText = 'position:fixed;bottom:8px;left:8px;right:8px;padding:8px;border-radius:10px;font-size:10px;text-align:center;z-index:99999;';
        if(status === 'SUBSCRIBED'){
          aviso.style.cssText += 'background:#1a3a1a;color:#8FAE7D;';
          aviso.textContent = '[Diagnóstico] Conectado ao tempo real com sucesso.';
        } else {
          aviso.style.cssText += 'background:#3a1a1a;color:#E2A33D;';
          aviso.textContent = '[Diagnóstico] Status da conexão em tempo real: ' + status;
        }
        document.body.appendChild(aviso);
      });
  } catch(erroDeRede){
    // Sem treino sincronizado por enquanto, continua com o que já está carregado localmente
    console.warn('Sem conexão pra buscar o treino sincronizado agora:', erroDeRede);
  }
}

function aplicarTreinoRecebidoDoSupabase(treinoRow){
  let a = alunasPersonal.find(function(x){ return x.nome === NOME_ALUNA_LOGADA; });
  if(!a){
    // Ela ainda não existia na lista local dessa sessão (comum no primeiro login dela, no aparelho dela) — cria agora
    a = { nome: NOME_ALUNA_LOGADA, status: 'ok', statusLabel: 'Ativa recente', nivel: 'Iniciante', freq: '3x por semana' };
    alunasPersonal.push(a);
  }
  a.treinoAtual = { fase: treinoRow.fase, volume: treinoRow.volume, dias: treinoRow.dias };
  dias = treinoRow.dias;
  const viewAtiva = document.querySelector('.view.active');
  if(viewAtiva && viewAtiva.getAttribute('data-view') === 'home' && typeof renderHome === 'function'){
    renderHome();
  }
}

async function salvarTreinoNoSupabase(alunaId, treinoAtual){
  const { data: linhaInserida, error: erroInsert } = await supabaseClient.from('treinos').insert({
    aluna_id: alunaId,
    fase: treinoAtual.fase,
    volume: treinoAtual.volume,
    dias: treinoAtual.dias
  }).select().single();

  if(erroInsert){
    mostrarConfirmacaoSalvamento(false, 'Erro ao salvar no banco: ' + erroInsert.message);
    return { sucesso: false, erro: erroInsert.message };
  }

  // Lê de volta do banco (não da memória local) pra confirmar que o dado real gravado bate com o que foi enviado
  const { data: leituraConfirmada, error: erroLeitura } = await supabaseClient.from('treinos').select('*').eq('id', linhaInserida.id).maybeSingle();
  if(erroLeitura || !leituraConfirmada){
    mostrarConfirmacaoSalvamento(false, 'Salvou, mas não consegui confirmar lendo de volta: ' + (erroLeitura ? erroLeitura.message : 'linha não encontrada'));
    return { sucesso: false, erro: 'falha na confirmação' };
  }

  const qtdExerciciosSalvos = leituraConfirmada.dias.reduce(function(soma, d){ return soma + (d.ex ? d.ex.length : 0); }, 0);
  mostrarConfirmacaoSalvamento(true, 'Confirmado no banco: ' + leituraConfirmada.dias.length + ' dias, ' + qtdExerciciosSalvos + ' exercícios, salvo às ' + new Date(leituraConfirmada.updated_at || Date.now()).toLocaleTimeString('pt-BR'));
  return { sucesso: true, dados: leituraConfirmada };
}

function mostrarConfirmacaoSalvamento(sucesso, texto){
  const el = document.createElement('div');
  el.style.cssText = 'position:fixed;bottom:8px;left:8px;right:8px;padding:10px;border-radius:10px;font-size:11px;text-align:center;z-index:999999;' +
    (sucesso ? 'background:#1a3a1a;color:#8FAE7D;' : 'background:#3a1a1a;color:#E2A33D;');
  el.textContent = (sucesso ? '✓ ' : '✗ ') + texto;
  document.body.appendChild(el);
  setTimeout(function(){ el.remove(); }, 8000);
}

const timersSalvamentoProgresso = {};

function salvarProgressoNoSupabase(nomeAluna){
  if(timersSalvamentoProgresso[nomeAluna]) clearTimeout(timersSalvamentoProgresso[nomeAluna]);
  timersSalvamentoProgresso[nomeAluna] = setTimeout(function(){
    delete timersSalvamentoProgresso[nomeAluna];
    executarSalvamentoProgresso(nomeAluna);
  }, 500);
}

async function executarSalvamentoProgresso(nomeAluna){
  if(!supabaseClient) return;
  try {
    const a = alunasPersonal.find(function(x){ return x.nome === nomeAluna; });
    if(!a || !a.email) return;
    const { data: alunaRow } = await supabaseClient.from('alunas').select('auth_id').eq('email', a.email).maybeSingle();
    if(!alunaRow || !alunaRow.auth_id) return; // sem login ainda, não tem onde salvar

    const pacote = {
      prog: getProgressoAluna(nomeAluna),
      seriesConcluidas: seriesConcluidas,
      diaDoSeriesConcluidas: detailDiaAtual
    };
    await supabaseClient.from('progresso_aluna').upsert({
      aluna_id: alunaRow.auth_id,
      dados: pacote,
      updated_at: new Date().toISOString()
    });
  } catch(erroDeRede){
    console.warn('Sem conexão agora, progresso ficou salvo só localmente por enquanto:', erroDeRede);
  }
}

async function carregarProgressoDoSupabase(authId, nomeAluna){
  if(!supabaseClient || !authId) return;
  try {
    const { data: linha } = await supabaseClient.from('progresso_aluna').select('dados').eq('aluna_id', authId).maybeSingle();
    if(!linha || !linha.dados) return;
    if(linha.dados.prog) progressoesPorAluna[nomeAluna] = linha.dados.prog;
    if(linha.dados.seriesConcluidas){
      seriesConcluidasSalvasNoBanco = linha.dados.seriesConcluidas;
      diaDasSeriesConcluidasSalvas = linha.dados.diaDoSeriesConcluidas;
    }
  } catch(erroDeRede){
    console.warn('Sem conexão pra carregar progresso salvo, começando com o que tiver localmente:', erroDeRede);
  }
}

function finalizarOnboarding(){
  setActive('launcher');
}

function finalizarOnboardingEIr(destino){
  setActive('launcher');
  if(destino === 'treino'){ openLevel2('home'); abrirListaDeTreinosDaSemana(); }
  else if(destino === 'dna'){ openLevel2('home'); openDetail('dna'); }
  else if(destino === 'sol'){ openLevel2('chatia'); }
}

let timerIntroAtual = null;

function pedirPermissaoNotificacao(){
  // Isso só guarda a permissão pro futuro (quando a Camada 2 de notificações reais estiver pronta).
  // Não envia notificação nenhuma agora — só evita ter que pedir de novo depois.
  if(typeof Notification === 'undefined') return; // navegador não suporta, não trava nada
  if(Notification.permission === 'default'){
    Notification.requestPermission().catch(function(){ /* usuária pode ter negado, sem problema */ });
  }
}

// ===== LEMBRAR LOGIN (opt-in, pra não ficar entrando sozinho na conta errada) =====
function getPreferenciaLembrarLogin(){
  try { return localStorage.getItem('musa_lembrar_login'); } catch(e){ return null; }
}

function salvarPreferenciaLembrarLogin(valor){
  try { localStorage.setItem('musa_lembrar_login', valor); } catch(e){}
}

function perguntarSeQuerLembrarLogin(){
  if(getPreferenciaLembrarLogin() !== null) return; // já respondeu antes, não pergunta de novo
  const modal = document.getElementById('modal-lembrar-login');
  if(modal) modal.style.display = 'flex';
}

async function responderLembrarLogin(salvar){
  salvarPreferenciaLembrarLogin(salvar ? 'sim' : 'nao');
  const modal = document.getElementById('modal-lembrar-login');
  if(modal) modal.style.display = 'none';
  // Se ela disse "não salvar", a sessão atual continua funcionando normalmente até ela sair ou fechar.
  // O que muda é só a PRÓXIMA vez que o app abrir: aí sim não vai entrar sozinho de novo (ver restaurarSessaoAtiva).
}

function confirmarSairDaConta(){
  if(confirm('Sair dessa conta? Você pode entrar de novo com outro login depois.')) sairDeVerdade();
}

function pularIntro(){
  if(timerIntroAtual){ clearTimeout(timerIntroAtual); timerIntroAtual = null; }
  setActive('onboarding');
}

// ===== INTRO DO DNA (mostra nas 10 primeiras vezes que a aluna abre o app, não só na primeira) =====
const LIMITE_ABERTURAS_COM_INTRO = 10;

function deveExibirEEIncrementarIntro(nomeAluna){
  const a = alunasPersonal.find(function(x){ return x.nome === nomeAluna; });
  if(!a) return false;
  a.contadorAberturas = (a.contadorAberturas || 0) + 1;
  salvarPerfilAlunaNoSupabase(nomeAluna);
  return a.contadorAberturas <= LIMITE_ABERTURAS_COM_INTRO;
}

function testarIntroNovamente(){
  sessaoTipo = 'aluna';
  document.getElementById('card-personal-launcher').style.display = 'none';
  setActive('intro');
  timerIntroAtual = setTimeout(function(){ setActive('onboarding'); }, 4000);
}

async function loginPersonal(){
  const email = document.getElementById('login-personal-email').value.trim().toLowerCase();
  const senha = document.getElementById('login-personal-senha').value;
  const erroEl = document.getElementById('login-personal-erro');

  // Acesso de teste garantido: sempre entra, mas agora ESPERA a sessão real terminar antes,
  // pra garantir que a busca de alunas novas funcione (isso depende de permissão do banco).
  if(email === EMAIL_PERSONAL.toLowerCase() && senha === SENHA_PERSONAL){
    erroEl.style.display = 'none';

    let statusSessao = 'Sem conexão com o Supabase (modo totalmente offline).';
    if(supabaseClient){
      try {
        const resultado = await supabaseClient.auth.signInWithPassword({ email: email, password: senha });
        statusSessao = resultado.error
          ? 'FALHOU: ' + resultado.error.message
          : 'OK, sessão real estabelecida.';
      } catch(erroDeRede){
        statusSessao = 'Erro de rede ao tentar: ' + erroDeRede.message;
      }
    }

    sessaoTipo = 'personal';
    document.getElementById('backbar').style.display = 'flex';
    document.getElementById('backlabel').textContent = 'Sair';
    pedirPermissaoNotificacao();
    openLevel2('personal');
    perguntarSeQuerLembrarLogin();

    // Diagnóstico único, grande, no topo da tela — impossível de não ver
    const diag = document.createElement('div');
    diag.id = 'diagnostico-login-completo';
    diag.style.cssText = 'position:fixed;top:0;left:0;right:0;background:' + (statusSessao.indexOf('OK') === 0 ? '#1a3a1a' : '#3a1a1a') + ';color:#fff;padding:14px;font-size:12px;z-index:999999;text-align:center;border-bottom:2px solid ' + (statusSessao.indexOf('OK') === 0 ? '#4a9;' : '#e55;');
    diag.innerHTML = '<b>Diagnóstico do login:</b> ' + statusSessao + '<br><span style="font-size:10px;opacity:0.8;cursor:pointer;text-decoration:underline;" onclick="this.parentElement.remove()">Fechar</span>';
    document.body.appendChild(diag);

    return;
  }

  if(!supabaseClient){
    if(email !== EMAIL_PERSONAL.toLowerCase() || senha !== SENHA_PERSONAL){
      erroEl.textContent = 'E-mail ou senha incorretos.';
      erroEl.style.display = 'block';
      return;
    }
    erroEl.style.display = 'none';
    sessaoTipo = 'personal';
    document.getElementById('backbar').style.display = 'flex';
    document.getElementById('backlabel').textContent = 'Sair';
    pedirPermissaoNotificacao();
    openLevel2('personal');
    perguntarSeQuerLembrarLogin();
    return;
  }

  try {
    let { data, error } = await supabaseClient.auth.signInWithPassword({ email: email, password: senha });

    if(error){
      if(email === EMAIL_PERSONAL.toLowerCase() && senha === SENHA_PERSONAL){
        // Primeira vez logando: cria a conta real do personal agora
        const cadastro = await supabaseClient.auth.signUp({ email: email, password: senha });
        if(cadastro.error){
          erroEl.textContent = cadastro.error.message;
          erroEl.style.display = 'block';
          return;
        }
        data = cadastro.data;
        await supabaseClient.from('perfis').insert({ id: data.user.id, tipo: 'personal', nome: 'Thiago' });
      } else {
        erroEl.textContent = 'E-mail ou senha incorretos.';
        erroEl.style.display = 'block';
        return;
      }
    }

    sessaoUsuarioAtual = data.user;
    erroEl.style.display = 'none';
    sessaoTipo = 'personal';
    document.getElementById('backbar').style.display = 'flex';
    document.getElementById('backlabel').textContent = 'Sair';
    pedirPermissaoNotificacao();
    openLevel2('personal');
    perguntarSeQuerLembrarLogin();
  } catch(erroDeRede){
    // Falha de rede de verdade — se as credenciais batem com as suas, entra local pra você continuar testando
    console.warn('Sem conexão com o Supabase agora, entrando localmente pra você continuar testando:', erroDeRede);
    if(email === EMAIL_PERSONAL.toLowerCase() && senha === SENHA_PERSONAL){
      erroEl.style.display = 'none';
      sessaoTipo = 'personal';
      document.getElementById('backbar').style.display = 'flex';
      document.getElementById('backlabel').textContent = 'Sair';
      pedirPermissaoNotificacao();
      openLevel2('personal');
      perguntarSeQuerLembrarLogin();
    } else {
      erroEl.textContent = 'E-mail ou senha incorretos.';
      erroEl.style.display = 'block';
    }
  }
}

function setActive(name){
  document.querySelectorAll('.view').forEach(function(v){ v.classList.remove('active'); });
  document.querySelector('[data-view="' + name + '"]').classList.add('active');
  try { localStorage.setItem('musaUltimaTela', JSON.stringify({ view: name, detailDia: (name === 'detail' ? detailDiaAtual : null) })); } catch(e){}
}

const abasFixasAluna = ['home', 'dados', 'mentoria', 'progresso', 'ranking', 'rodadavida'];

function openLevel2(which){
  level2 = which;
  setActive(which);

  const ehAbaFixaDaAluna = sessaoTipo === 'aluna' && abasFixasAluna.indexOf(which) !== -1;
  document.getElementById('backbar').style.display = 'flex';
  document.getElementById('top-logo-fixa').style.display = 'none';
  document.getElementById('bottom-nav-fixa').style.display = ehAbaFixaDaAluna ? 'flex' : 'none';
  if(ehAbaFixaDaAluna) atualizarNavAtiva(which);

  document.getElementById('backlabel').textContent = (which === 'personal') ? 'Sair' : 'Voltar para o início';
  const phoneEl = document.getElementById('phone-container');
  if(phoneEl) phoneEl.classList.toggle('modo-personal', which === 'personal');
  if(which === 'personal'){ showPersonalView('dashboard'); carregarCatalogoPersonal(); }
  if(which === 'chatia'){ inicializarChatIA(); }
  if(which === 'home' && typeof renderHome === 'function'){ renderHome(); }
  if(which === 'dados' && typeof renderAvaliacoes === 'function'){ renderAvaliacoes(); }
  if(which === 'progresso' && typeof renderMeuProgresso === 'function'){ renderMeuProgresso(); }
  if(which === 'ranking' && typeof renderRanking === 'function'){ renderRanking(); }
  if(which === 'rodadavida'){ renderRodaDaVidaAluna(); }
}

function atualizarNavAtiva(which){
  document.querySelectorAll('.bottom-nav-item').forEach(function(el){
    el.classList.toggle('ativo', el.getAttribute('data-nav') === which);
  });
}

function irParaAbaFixa(which){
  openLevel2(which);
}

/* ===== CHAT DE IA REAL ===== */
/* ===== SUPABASE — conexão real com o banco de dados ===== */
const SUPABASE_URL = 'https://vwiaszatrphqitxlefmz.supabase.co';
const LINK_DO_APP = 'https://dna-musa-teamfernandes.vercel.app/'; // domínio fixo real, não depende de onde o app está rodando (evita link errado ao testar localmente)
const SUPABASE_ANON_KEY = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InZ3aWFzemF0cnBocWl0eGxlZm16Iiwicm9sZSI6ImFub24iLCJpYXQiOjE3ODUzMzgyNzgsImV4cCI6MjEwMDkxNDI3OH0.Cz0tZr0uhS5sWUgkXmNBZCQzZL6pz_j2D7fq0jW1H6Y';
const supabaseClient = (typeof window.supabase !== 'undefined') ? window.supabase.createClient(SUPABASE_URL, SUPABASE_ANON_KEY) : null;
let sessaoUsuarioAtual = null; // guarda o usuário logado de verdade (Supabase Auth)
let NOME_ALUNA_LOGADA = 'Andriele Caroline Rubert'; // padrão pra testes locais; some real, vira dinâmico após login de verdade

async function restaurarSessaoAtiva(){
  function mostrarDiagnostico(texto, ehErro){
    const el = document.createElement('div');
    el.style.cssText = 'position:fixed;top:14px;left:8px;right:8px;background:' + (ehErro ? '#3a1a1a' : '#1a2a3a') + ';color:' + (ehErro ? '#E2A33D' : '#9ec5e8') + ';padding:8px;border-radius:8px;font-size:10px;text-align:center;z-index:999998;';
    el.textContent = '[Sessão] ' + texto;
    document.body.appendChild(el);
    setTimeout(function(){ el.remove(); }, 9000);
  }

  if(!supabaseClient){ mostrarDiagnostico('supabaseClient não existe (CDN não carregou).', true); return; }

  // Se ela já disse explicitamente que não quer login salvo, desloga e mostra a tela normal,
  // mesmo que o Supabase ainda tenha uma sessão guardada no navegador.
  if(getPreferenciaLembrarLogin() === 'nao'){
    try { await supabaseClient.auth.signOut(); } catch(e){}
    mostrarDiagnostico('Login não fica salvo nesse aparelho (preferência da usuária). Mostrando tela normal.', false);
    return;
  }

  try {
    const { data: sessaoData, error: erroSessao } = await supabaseClient.auth.getSession();
    if(erroSessao){ mostrarDiagnostico('Erro ao buscar sessão: ' + erroSessao.message, true); return; }
    const sessao = sessaoData ? sessaoData.session : null;
    if(!sessao || !sessao.user){ mostrarDiagnostico('Nenhuma sessão salva encontrada (normal se for primeiro acesso ou já saiu antes).', false); return; }

    // Verificação rígida extra: exige token de acesso real e não expirado, não confia só no objeto de sessão existir
    if(!sessao.access_token){ mostrarDiagnostico('Sessão sem token de acesso válido, ignorando.', false); return; }
    if(sessao.expires_at && (sessao.expires_at * 1000) < Date.now()){ mostrarDiagnostico('Sessão salva já expirou, ignorando.', false); return; }

    mostrarDiagnostico('Sessão encontrada, usuário: ' + sessao.user.email, false);
    sessaoUsuarioAtual = sessao.user;
    const { data: perfilRow, error: erroPerfil } = await supabaseClient.from('perfis').select('tipo, nome').eq('id', sessao.user.id).maybeSingle();
    if(erroPerfil){ mostrarDiagnostico('Erro ao buscar perfil: ' + erroPerfil.message, true); return; }
    if(!perfilRow){ mostrarDiagnostico('Sessão existe, mas não achou linha em "perfis" pra esse usuário.', true); return; }

    mostrarDiagnostico('Perfil encontrado: tipo=' + perfilRow.tipo + ', nome=' + perfilRow.nome, false);

    if(perfilRow.tipo === 'personal'){
      sessaoTipo = 'personal';
      const backbar = document.getElementById('backbar');
      if(backbar) backbar.style.display = 'flex';
      const backlabel = document.getElementById('backlabel');
      if(backlabel) backlabel.textContent = 'Sair';
      pedirPermissaoNotificacao();
      openLevel2('personal');
      perguntarSeQuerLembrarLogin();
    } else if(perfilRow.tipo === 'aluna'){
      NOME_ALUNA_LOGADA = perfilRow.nome;
      const { data: alunaRow } = await supabaseClient.from('alunas').select('*').eq('auth_id', sessao.user.id).maybeSingle();
      let alunaLocal = alunasPersonal.find(function(x){ return x.nome === NOME_ALUNA_LOGADA; });
      if(!alunaLocal && alunaRow){
        alunaLocal = {
          nome: alunaRow.nome, email: alunaRow.email, telefone: alunaRow.telefone || '',
          nivel: alunaRow.nivel || 'Iniciante', freq: alunaRow.freq || '3x por semana',
          piramide: alunaRow.piramide || '', objetivo: alunaRow.objetivo || '',
          restricoes: alunaRow.restricoes || 'Nenhuma relatada', academia: alunaRow.academia || '',
          idade: alunaRow.idade || null, dataAnamnese: alunaRow.data_anamnese || new Date().toISOString().slice(0,10),
          status: 'ok', statusLabel: 'Ativa recente'
        };
        if(alunaRow.dados_extras && typeof alunaRow.dados_extras === 'object'){
          Object.keys(alunaRow.dados_extras).forEach(function(campo){
            if(alunaRow.dados_extras[campo] != null) alunaLocal[campo] = alunaRow.dados_extras[campo];
          });
        }
        alunasPersonal.push(alunaLocal);
      }
      if(alunaLocal && alunaLocal.treinoAtual && alunaLocal.treinoAtual.dias){
        dias = alunaLocal.treinoAtual.dias;
      }
      aplicarIdentidadeVisual(NOME_ALUNA_LOGADA);
      pedirPermissaoNotificacao();
      sessaoTipo = 'aluna';
      const cardPersonal = document.getElementById('card-personal-launcher');
      if(cardPersonal) cardPersonal.style.display = 'none';
      await carregarELigarTreinoDaAluna(sessao.user.id);
      mostrarDiagnostico('Restaurada com sucesso, indo pra tela dela.', false);

      const dentroDoLimiteDeIntroRestaurada = deveExibirEEIncrementarIntro(NOME_ALUNA_LOGADA);
      if(dentroDoLimiteDeIntroRestaurada){
        setActive('intro');
        timerIntroAtual = setTimeout(function(){ setActive('launcher'); }, 4000);
        perguntarSeQuerLembrarLogin();
        return;
      }

      // Volta exatamente pra tela que ela estava, não sempre pro launcher
      let telaSalva = null;
      try { telaSalva = JSON.parse(localStorage.getItem('musaUltimaTela') || 'null'); } catch(e){}
      const telasValidasParaAluna = ['home', 'launcher', 'dados', 'mentoria', 'chatia', 'detail'];
      if(telaSalva && telasValidasParaAluna.indexOf(telaSalva.view) !== -1){
        openLevel2('home'); // garante dados/dias carregados antes de qualquer tela específica
        if(telaSalva.view === 'detail' && telaSalva.detailDia != null && dias[telaSalva.detailDia]){
          openDetail('dia', telaSalva.detailDia);
        } else if(telaSalva.view !== 'home'){
          setActive(telaSalva.view);
        }
      } else {
        setActive('launcher');
      }
      perguntarSeQuerLembrarLogin();
    }
  } catch(erroDeRede){
    mostrarDiagnostico('Erro de conexão: ' + erroDeRede.message, true);
  }
}
restaurarSessaoAtiva();

const TELEFONE_PERSONAL = '5500000000000'; // ⚠️ SUBSTITUA pelo seu WhatsApp real, formato 55DDDNÚMERO
let historicoChatIA = [];

function montarContextoAluna(){
  const nome = NOME_ALUNA_LOGADA;
  const a = alunasPersonal.find(function(x){ return x.nome === nome; });
  if(!a) return '';
  const stats = calcularEstatisticasAluna(nome);
  const prog = getProgressoAluna(nome);
  let contexto = 'DADOS DA ALUNA ATUAL (use isso pra personalizar, nunca invente dado que não está aqui):\n';
  contexto += '- Nome: ' + a.nome.split(' ')[0] + '\n';
  contexto += '- Nível: ' + a.nivel + '\n';
  contexto += '- Fase atual: ' + (a.treinoAtual ? a.treinoAtual.fase : 'não definida') + '\n';
  contexto += '- Semana atual do ciclo: ' + prog.semana + '\n';
  if(stats.temDados){
    contexto += '- Constância: ' + stats.totalConcluidos + '/' + stats.totalPlanejado + ' treinos\n';
  } else {
    contexto += '- Ainda no primeiro dia, DNA MUSA calibrando\n';
  }
  if(a.restricoes) contexto += '- Restrição relatada: ' + a.restricoes + '\n';
  return contexto;
}

function montarSystemPromptChatIA(){
  return 'Você é Sol, do time do DNA Musa, extensão do professor Thiago Fernandes. Especialista em treinamento feminino, hipertrofia, fisiologia do exercício, biomecânica e cinesiologia.\n\n' +
    'IMPORTANTE: nunca se identifique como Inteligência Artificial, IA, robô, chatbot ou assistente virtual, mesmo se a aluna perguntar diretamente. Se ela perguntar "você é uma IA?", responda de forma natural e acolhedora, redirecionando pro seu papel de te ajudar, sem confirmar nem negar de forma técnica.\n\n' +
    'COMUNICAÇÃO: direta, simples, sem jargão desnecessário, sempre traduzindo ciência pra linguagem acessível. Tom motivador, próximo, humano, sério e profissional. Sempre explique o "porquê" das coisas.\n\n' +
    'REGRAS DE ESCRITA (inegociáveis):\n' +
    '- NUNCA use travessões (—) em nenhuma resposta. Use vírgulas e pontos normalmente.\n' +
    '- Sem gírias de brincadeira, sem emojis, sem exclamações forçadas tipo "Bora lá!". Tom sério, direto, acolhedor, mas nunca "engraçadinho".\n\n' +
    'REGRAS DE CONTEÚDO (inegociáveis):\n' +
    '1. Você NUNCA substitui um exercício da prescrição sozinha, nem muda carga/série definitiva. Isso sempre depende da aprovação do personal.\n' +
    '2. Você PODE sugerir uma variação momentânea pro mesmo grupamento muscular, mas deixe claro que é uma sugestão pontual, não uma mudança definitiva.\n' +
    '3. NUNCA diga frases como "vou falar com o Thiago" ou "vou perguntar pro personal". Fale de forma natural, como se você mesma fosse parte da equipe.\n' +
    '4. Nunca diagnostique lesões ou condições médicas. Se a aluna relatar dor, oriente com cautela e sugira avaliação presencial quando fizer sentido, sem alarmismo.\n' +
    '5. Nunca dê conselho nutricional prescritivo (dieta, macros). Isso é do nutricionista, você pode falar de princípios gerais só.\n' +
    '6. NUNCA explique que variamos exercícios ou métodos "pra não parecer sempre a mesma coisa", "pra não enjoar da rotina" ou qualquer menção a monotonia/rotina como motivo de uma escolha. Isso nunca deve ser mencionado, em nenhum contexto. Se perguntarem por que o treino mudou, responda apenas que variamos exercícios dentro do mesmo padrão de movimento pra continuar estimulando o corpo da forma adequada, de acordo com a necessidade da aluna e a estratégia do treino, sem qualquer menção a variedade por si só.\n' +
    '7. NUNCA sugira "carga leve" para exercício nenhum. Sempre carga moderada a pesada. Isso não é fisioterapia (que trabalha sem carga, pra ganhar amplitude e recuperar gestos funcionais), é treinamento pra evoluir, ganhar músculo, força e resistência.\n' +
    '8. Exercícios que trabalham o mesmo grupo muscular (ex: Cadeira Flexora e Cama Flexora) PODEM aparecer no mesmo treino como exercícios separados. A única restrição é não combiná-los juntos num bi-set/método combinado, nunca sobre coexistirem na mesma sessão.\n' +
    '9. Ao explicar uma redução de carga sugerida, nunca use a palavra "respirar/respiro". Explique que foi um ajuste inteligente, baseado no que aconteceu na 1ª série daquele exercício, cruzado com os feedbacks e dados que o DNA MUSA reúne.\n' +
    '10. Nunca use a palavra "fresca" pra descrever a 1ª série de um exercício. Diga que é a "série inicial", a que serve de base pra aplicar a metodologia e calcular a progressão.\n' +
    '11. Ao explicar bi-set, sempre mencione que ele otimiza o tempo de treino, maximiza os ganhos, e potencializa o gasto calórico.\n' +
    '12. Ao explicar o Bloco de Choque, nunca mencione "sacudir o estímulo" ou "rotina". Explique que as repetições caem pra permitir mais intensidade e carga, o descanso aumenta pra recuperar os substratos energéticos, e que mesmo não sendo treino de força pura, a força aumenta naturalmente, junto com desempenho, ganho de massa muscular e gasto energético.\n' +
    '13. Se a pergunta for muito específica, pessoal, exigir avaliação presencial, ou você genuinamente não tiver informação suficiente pra responder com confiança, diga de forma acolhedora que vai confirmar esse detalhe com mais cuidado e retornar em breve. Nunca diga "vou falar com o Thiago" nem "vou perguntar pro personal", apenas "vou confirmar isso com calma e te retorno". Nesses casos, e SOMENTE nesses casos, termine sua resposta com a marcação exata [[SINALIZAR_PERSONAL]] na última linha (isso não aparece pra aluna, é só pro sistema identificar).\n\n' +
    '14. Se a aluna perguntar especificamente sobre a EXECUÇÃO de um exercício (como fazer, forma certa, passo a passo, não sobre quais músculos ativa), identifique o nome exato do exercício e termine sua resposta com a marcação [[VIDEO: Nome Exato do Exercício]] na última linha. Isso mostra o vídeo real pra ela automaticamente, então sua explicação em texto deve ser breve, só complementando o vídeo, não descrevendo a execução passo a passo por escrito.\n\n' +
    '15. Se a aluna relatar sinais reais de sofrimento emocional significativo (tristeza profunda, desesperança, ansiedade muito intensa, menções de desistir de tudo, isolamento, ou qualquer sinal de risco à própria segurança), responda sempre com acolhimento genuíno primeiro, nunca ignore o que ela disse pra falar só de treino. Não tente diagnosticar nem fazer terapia. Depois de responder com acolhimento, termine sua resposta com a marcação exata [[RISCO_EMOCIONAL]] na última linha (isso não aparece pra aluna, é só pro sistema identificar e avisar o personal, sem revelar o conteúdo da conversa pra ele). Não use essa marcação pra reclamações comuns de cansaço, desânimo pontual com o treino ou frustração passageira, só pra sinais reais de sofrimento.\n\n' +
    montarContextoAluna();
}

function inicializarChatIA(){
  if(historicoChatIA.length > 0) return; // já tem conversa rolando, não reseta
  const nomeCurto = NOME_ALUNA_LOGADA.split(' ')[0];
  historicoChatIA = [];
  renderMensagensChatIA();
  adicionarMensagemChatIA('ia', 'Oi, ' + nomeCurto + '! Eu sou a Sol, aqui do time do DNA Musa. Estou aqui pra te ajudar a entender melhor seu treino, tirar dúvidas sobre exercícios ou o que precisar. Pode perguntar!');
}

function adicionarMensagemChatIA(autor, texto){
  historicoChatIA.push({ autor: autor, texto: texto });
  renderMensagensChatIA();
}

function renderMensagensChatIA(){
  const container = document.getElementById('chatia-mensagens');
  if(!container) return;
  container.innerHTML = historicoChatIA.map(function(m){
    const ehAluna = m.autor === 'aluna';
    return '<div style="align-self:' + (ehAluna ? 'flex-end' : 'flex-start') + ';max-width:80%;background:' + (ehAluna ? 'var(--gold-deep)' : 'var(--card-2)') + ';color:' + (ehAluna ? '#fff' : 'var(--text)') + ';padding:10px 14px;border-radius:14px;font-size:13px;line-height:1.4;">' + m.texto.replace(/\n/g, '<br>') + '</div>';
  }).join('');
  container.scrollTop = container.scrollHeight;
}

async function enviarMensagemChatIA(){
  const input = document.getElementById('chatia-input');
  const texto = input.value.trim();
  if(!texto) return;
  input.value = '';
  adicionarMensagemChatIA('aluna', texto);

  const statusEl = document.getElementById('chatia-status');
  statusEl.textContent = 'Digitando...';

  const mensagensParaAPI = historicoChatIA.map(function(m){
    return { role: m.autor === 'aluna' ? 'user' : 'assistant', content: m.texto };
  });

  try {
    const response = await fetch(SUPABASE_URL + '/functions/v1/chat-sol', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json', 'apikey': SUPABASE_ANON_KEY, 'Authorization': 'Bearer ' + SUPABASE_ANON_KEY },
      body: JSON.stringify({
        system: montarSystemPromptChatIA(),
        messages: mensagensParaAPI
      })
    });
    const data = await response.json();
    if(!response.ok || data.error){
      throw new Error('HTTP ' + response.status + ': ' + (data.error && data.error.message ? data.error.message : JSON.stringify(data)));
    }
    let textoResposta = (data.content || []).map(function(c){ return c.text || ''; }).join('\n').trim();
    statusEl.textContent = '';

    const precisaSinalizar = textoResposta.indexOf('[[SINALIZAR_PERSONAL]]') !== -1;
    if(precisaSinalizar){
      textoResposta = textoResposta.replace('[[SINALIZAR_PERSONAL]]', '').trim();
      registrarDuvidaSinalizada(texto, textoResposta);
    }

    const precisaSinalizarRisco = textoResposta.indexOf('[[RISCO_EMOCIONAL]]') !== -1;
    if(precisaSinalizarRisco){
      textoResposta = textoResposta.replace('[[RISCO_EMOCIONAL]]', '').trim();
      sinalizarRiscoEmocional(NOME_ALUNA_LOGADA);
    }

    const matchVideo = textoResposta.match(/\[\[VIDEO:\s*([^\]]+)\]\]/);
    if(matchVideo){
      textoResposta = textoResposta.replace(matchVideo[0], '').trim();
    }

    adicionarMensagemChatIA('ia', textoResposta || 'Desculpa, não consegui responder agora, tenta de novo em instantes.');

    if(matchVideo){
      const nomeExVideo = matchVideo[1].trim();
      mostrarVideoNoChatIA(nomeExVideo, texto);
    }
  } catch(erro) {
    statusEl.textContent = '';
    console.error('[Chat Sol] erro real:', erro);
    adicionarMensagemChatIA('ia', 'Não consegui responder agora. Enquanto isso, manda sua dúvida direto pro seu personal, ele te responde rapidinho.');
  }
}

function mostrarVideoNoChatIA(nomeExercicio, perguntaOriginal){
  const exBanco = buscarExercicioNoBanco(nomeExercicio);
  const embed = exBanco ? getEmbedUrl(exBanco.video) : null;
  const container = document.getElementById('chatia-mensagens');
  if(!container) return;

  const videoDiv = document.createElement('div');
  videoDiv.style.cssText = 'align-self:flex-start;max-width:90%;width:100%;';
  videoDiv.innerHTML = embed
    ? '<div class="video-block" style="margin:4px 0;"><iframe src="' + embed + '" title="' + nomeExercicio + '" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></div>'
    : '<p style="font-size:12px;color:var(--text-faint);margin:4px 0;">Vídeo desse exercício ainda não está disponível.</p>';
  container.appendChild(videoDiv);

  setTimeout(function(){
    const perguntaDiv = document.createElement('div');
    perguntaDiv.style.cssText = 'align-self:flex-start;max-width:80%;background:var(--card-2);color:var(--text);padding:10px 14px;border-radius:14px;font-size:13px;line-height:1.4;';
    perguntaDiv.textContent = 'Ficou claro, ou ainda ficou com dúvida?';
    container.appendChild(perguntaDiv);

    const botoesDiv = document.createElement('div');
    botoesDiv.style.cssText = 'align-self:flex-start;display:flex;gap:8px;margin-top:-4px;';
    botoesDiv.innerHTML =
      '<button class="btn-gold" style="width:auto;padding:8px 14px;margin:0;font-size:12px;" onclick="this.parentElement.remove()">Ficou claro!</button>' +
      '<button class="btn-gold" style="width:auto;padding:8px 14px;margin:0;font-size:12px;background:var(--card-2);color:var(--gold-soft);border:1px solid var(--border);" onclick="escalarParaPersonal(\'' + perguntaOriginal.replace(/'/g,"\\'") + '\', this)">Ainda não entendi</button>';
    container.appendChild(botoesDiv);
    container.scrollTop = container.scrollHeight;
  }, 400);
}

function escalarParaPersonal(perguntaOriginal, botaoEl){
  const alunaAtual = alunasPersonal.find(function(a){ return a.nome === NOME_ALUNA_LOGADA; });
  const nomeCurto = alunaAtual ? alunaAtual.nome.split(' ')[0] : 'aluna';
  const mensagem = encodeURIComponent('Oi Thiago! A ' + nomeCurto + ' ainda ficou com dúvida sobre: "' + perguntaOriginal + '". Pode dar uma olhada?');
  const linkWpp = 'https://wa.me/' + TELEFONE_PERSONAL + '?text=' + mensagem;
  botaoEl.parentElement.innerHTML = '<a class="btn-gold" style="width:auto;padding:8px 14px;margin:0;font-size:12px;background:#25D366;color:#fff;text-decoration:none;" href="' + linkWpp + '" target="_blank" rel="noopener"><i class="ti ti-brand-whatsapp" style="vertical-align:-2px;margin-right:4px;"></i>Chamar no WhatsApp</a>';
  if(alunaAtual) registrarDuvidaSinalizada(perguntaOriginal, '(dúvida de execução, aluna não entendeu pelo vídeo)');
}

function registrarDuvidaSinalizada(pergunta, respostaSol){
  const alunaAtual = alunasPersonal.find(function(a){ return a.nome === NOME_ALUNA_LOGADA; });
  if(!alunaAtual) return;
  if(!alunaAtual.duvidasSinalizadas) alunaAtual.duvidasSinalizadas = [];
  alunaAtual.duvidasSinalizadas.push({ pergunta: pergunta, respostaSol: respostaSol, resolvida: false, data: new Date().toISOString() });
}

// ===== ALERTA DE RISCO EMOCIONAL (via Sol) =====
// Guarda só a data do sinal e se o personal já viu. NUNCA guarda o conteúdo da conversa aqui,
// a conversa com a Sol é privada da aluna, só o sinal de "preciso de atenção" chega pro personal.
function sinalizarRiscoEmocional(nomeAluna){
  const a = alunasPersonal.find(function(x){ return x.nome === nomeAluna; });
  if(!a) return;
  a.sinalRiscoEmocional = { data: new Date().toISOString(), visto: false };
  salvarPerfilAlunaNoSupabase(nomeAluna);
}

function marcarSinalRiscoComoVisto(nomeAluna){
  const a = alunasPersonal.find(function(x){ return x.nome === nomeAluna; });
  if(!a || !a.sinalRiscoEmocional) return;
  a.sinalRiscoEmocional.visto = true;
  salvarPerfilAlunaNoSupabase(nomeAluna);
  renderAlunas();
}

function renderAlertasRiscoEmocional(){
  const el = document.getElementById('alertas-risco-emocional-area');
  if(!el) return;
  const alunasComSinal = alunasPersonal.filter(function(a){ return a.sinalRiscoEmocional && !a.sinalRiscoEmocional.visto; });
  if(alunasComSinal.length === 0){ el.innerHTML = ''; return; }
  el.innerHTML = alunasComSinal.map(function(a){
    return '<div class="info-box" style="border-color:var(--gold-soft);margin-bottom:10px;display:flex;justify-content:space-between;align-items:flex-start;gap:10px;">' +
      '<div><p class="lbl" style="margin-bottom:4px;">💛 Identificamos sinais de que ' + a.nome + ' pode estar passando por um momento difícil.</p>' +
      '<p class="txt" style="font-size:11px;color:var(--text-faint);">Considere entrar em contato para oferecer apoio. Por privacidade, o conteúdo da conversa com a Sol não é compartilhado.</p></div>' +
      '<span class="acao-pill" style="flex-shrink:0;" onclick="marcarSinalRiscoComoVisto(\'' + a.nome.replace(/'/g,"\\'") + '\')">Marcar como visto</span>' +
    '</div>';
  }).join('');
}

// ===== RESUMO DE PERFORMANCE POR ALUNA, GERADO POR IA (painel do Personal) =====
function montarDadosParaResumoIA(nomeAluna){
  const a = alunasPersonal.find(function(x){ return x.nome === nomeAluna; });
  if(!a) return '';
  const prog = getProgressoAluna(nomeAluna);

  const totalDiasSemana = totalDiasDeTreino();
  const concluidosSemana = (prog.diasConcluidos[prog.semana] || []).length;

  const hoje = new Date();
  const seteDiasAtras = new Date(hoje); seteDiasAtras.setDate(hoje.getDate() - 7);
  const checkins = Object.values(prog.checkinsEmocionais || {}).filter(function(c){ return new Date(c.data + 'T00:00:00') >= seteDiasAtras; });
  function media(campo){
    const valores = checkins.map(function(c){ return c[campo]; }).filter(function(v){ return v != null; });
    if(valores.length === 0) return null;
    return Math.round((valores.reduce(function(s,v){ return s+v; }, 0) / valores.length) * 10) / 10;
  }
  const humorMedio = media('humor'), ansiedadeMedia = media('ansiedade'), disposicaoMedia = media('disposicao'), sonoMedioHoras = media('sonoHoras');

  const diasComHabitosNoPeriodo = Object.keys(prog.habitosDoDia || {}).filter(function(d){ return new Date(d + 'T00:00:00') >= seteDiasAtras; });
  const totalHabitosMarcados = diasComHabitosNoPeriodo.reduce(function(soma, d){
    return soma + CATALOGO_HABITOS_DIARIOS.filter(function(h){ return prog.habitosDoDia[d][h.id]; }).length;
  }, 0);

  let texto = 'Dados da aluna ' + a.nome + ' para gerar o resumo de performance:\n\n';
  texto += '- Status do plano: ' + statusDoPlano(a) + '\n';
  texto += '- Nível: ' + (a.nivel || 'não informado') + ', frequência desejada: ' + (a.freq || 'não informada') + '\n';
  texto += '- Treinos concluídos essa semana: ' + concluidosSemana + ' de ' + totalDiasSemana + ' planejados\n';
  texto += '- Check-ins de bem-estar nos últimos 7 dias: ' + checkins.length + ' registrados\n';
  if(humorMedio != null) texto += '  - Humor médio: ' + humorMedio + '/5\n';
  if(ansiedadeMedia != null) texto += '  - Ansiedade média: ' + ansiedadeMedia + '/5\n';
  if(disposicaoMedia != null) texto += '  - Disposição média: ' + disposicaoMedia + '/5\n';
  if(sonoMedioHoras != null) texto += '  - Sono médio: ' + sonoMedioHoras + 'h por noite\n';
  texto += '- Hábitos diários marcados nos últimos 7 dias: ' + totalHabitosMarcados + ' de ' + (diasComHabitosNoPeriodo.length * CATALOGO_HABITOS_DIARIOS.length) + ' possíveis\n';
  if(a.composicaoAtual && a.composicaoAtual.peso != null) texto += '- Peso atual: ' + a.composicaoAtual.peso + 'kg' + (a.composicaoAtual.gordura != null ? ', percentual de gordura: ' + a.composicaoAtual.gordura + '%' : '') + '\n';
  if(a.restricoes) texto += '- Restrições relatadas: ' + a.restricoes + '\n';

  return texto;
}

async function gerarResumoPerformanceAluna(nomeAluna){
  const area = document.getElementById('resumo-ia-area');
  if(!area) return;
  area.innerHTML = '<div class="info-box"><p class="txt" style="color:var(--text-faint);">Gerando resumo com IA...</p></div>';

  const dados = montarDadosParaResumoIA(nomeAluna);
  const systemPrompt = 'Você ajuda um personal trainer a entender rapidamente a performance de uma aluna, a partir dos dados que ele te passar. ' +
    'Escreva em português, tom profissional e direto, sem travessões (use vírgulas e pontos), sem emojis. ' +
    'Estruture a resposta em markdown com estes títulos, nessa ordem: **Panorama**, **Pontos fortes**, **Pontos de atenção**, **Sugestão de conduta para a próxima semana**. ' +
    'Use apenas os dados fornecidos, não invente números. Se um dado não foi informado, não mencione ele.';

  try {
    const response = await fetch(SUPABASE_URL + '/functions/v1/chat-sol', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json', 'apikey': SUPABASE_ANON_KEY, 'Authorization': 'Bearer ' + SUPABASE_ANON_KEY },
      body: JSON.stringify({
        system: systemPrompt,
        messages: [{ role: 'user', content: dados }]
      })
    });
    const data = await response.json();
    if(!response.ok || data.error){
      throw new Error('HTTP ' + response.status + ': ' + (data.error && data.error.message ? data.error.message : JSON.stringify(data)));
    }
    const textoResumo = (data.content || []).map(function(c){ return c.text || ''; }).join('\n').trim();
    const idTextoEscapado = 'resumo-ia-texto-' + nomeAluna.replace(/[^a-zA-Z0-9]/g,'');
    area.innerHTML = '<div class="info-box">' +
      '<p id="' + idTextoEscapado + '" class="txt" style="white-space:pre-wrap;">' + (textoResumo || 'Não consegui gerar o resumo agora, tenta de novo.') + '</p>' +
      '<button class="btn-gold" style="background:var(--card-2);color:var(--gold-soft);border:1px solid var(--border);margin-top:8px;" onclick="copiarResumoIA(\'' + idTextoEscapado + '\')">Copiar texto</button>' +
    '</div>';
  } catch(erro){
    console.error('[Resumo IA] erro real:', erro);
    area.innerHTML = '<div class="info-box"><p class="txt" style="color:#E2A33D;">Não consegui gerar o resumo agora. Tenta de novo em instantes.</p></div>';
  }
}

function copiarResumoIA(idTexto){
  const el = document.getElementById(idTexto);
  if(!el) return;
  navigator.clipboard.writeText(el.textContent).then(function(){
    const btn = event.target;
    const textoOriginal = btn.textContent;
    btn.textContent = 'Copiado!';
    setTimeout(function(){ btn.textContent = textoOriginal; }, 1500);
  });
}

function buscarExercicioNoBanco(nomeEx){
  const nomeUpper = nomeEx.toUpperCase().trim();
  let ex = exerciciosBanco.find(function(e){ return e.nome.toUpperCase() === nomeUpper; });
  if(ex) return ex;
  // Remove variações entre parênteses, ex: "Leg Press 45 (pés altos)" -> "Leg Press 45"
  const nomeSemParenteses = nomeUpper.replace(/\s*\([^)]*\)\s*/g, '').trim();
  ex = exerciciosBanco.find(function(e){ return e.nome.toUpperCase() === nomeSemParenteses; });
  if(ex) return ex;
  // Tenta por correspondência de início (um nome contém o outro)
  ex = exerciciosBanco.find(function(e){
    const b = e.nome.toUpperCase();
    return b.indexOf(nomeSemParenteses) === 0 || nomeSemParenteses.indexOf(b) === 0;
  });
  return ex || null;
}

let cronometroInterval = null;
let cronometroSegundos = 0;
let cronometroRodando = false;

let descansoIntervals = {};

let seriesConcluidas = {};
let seriesConcluidasSalvasNoBanco = null;
let diaDasSeriesConcluidasSalvas = null;

function alternarExercicioExpandido(idx){
  const container = document.getElementById('ex-expandido-' + idx);
  if(!container) return;
  container.style.display = container.style.display === 'none' ? 'block' : 'none';
}

function atualizarBarraProgressoDia(d){
  d.ex.forEach(function(_, idx){
    const segmento = document.querySelector('.segmento-progresso[data-idx="' + idx + '"]');
    if(!segmento) return;
    const series = seriesConcluidas[idx];
    const concluido = series && series.length > 0 && series.every(function(v){ return v; });
    segmento.style.background = concluido ? 'var(--success)' : 'var(--border)';
  });
}

function alternarSubstituicoesInline(idx, nomeEx){
  const container = document.getElementById('subs-inline-' + idx);
  if(!container) return;
  if(container.style.display === 'none' || !container.innerHTML){
    const exAtual = buscarExercicioNoBanco(nomeEx);
    const grupo = exAtual ? (exAtual.grupo || exAtual.categoria) : null;
    const opcoes = grupo ? exerciciosBanco.filter(function(e){ return (e.grupo === grupo || e.categoria === grupo) && e.nome.toUpperCase() !== nomeEx.toUpperCase(); }).slice(0, 3) : [];
    container.innerHTML = opcoes.length === 0
      ? '<p class="txt" style="font-size:11px;color:var(--text-faint);margin:0 0 6px;">Sem opções parecidas cadastradas ainda.</p>'
      : '<div class="info-box" style="margin:0 0 8px;padding:10px 12px;">' +
          '<p class="lbl" style="margin-bottom:6px;">Opções parecidas (mesma região)</p>' +
          opcoes.map(function(e){ return '<p class="txt" style="margin-bottom:4px;">· ' + capitalizarNomeExercicio(e.nome) + '</p>'; }).join('') +
          '<p class="txt" style="font-size:10.5px;color:var(--text-faint);margin:6px 0 0;">Isso é só pra referência, seu treino oficial continua o mesmo até seu personal ajustar.</p>' +
        '</div>';
    container.style.display = 'block';
  } else {
    container.style.display = 'none';
  }
}

function concluirSerie(idx, serieIdx, segundosDescanso){
  if(!seriesConcluidas[idx]) return;
  seriesConcluidas[idx][serieIdx] = !seriesConcluidas[idx][serieIdx];
  const marcandoAgora = seriesConcluidas[idx][serieIdx];
  if(marcandoAgora){
    iniciarDescanso(idx, segundosDescanso);
  }
  salvarProgressoNoSupabase(NOME_ALUNA_LOGADA);
  const todasFeitas = seriesConcluidas[idx].every(function(v){ return v; });
  openDetail('dia', detailDiaAtual);

  const pill = document.getElementById('serie-pill-' + idx + '-' + serieIdx);
  if(pill && marcandoAgora){
    pill.classList.add('pulse');
    setTimeout(function(){ pill.classList.remove('pulse'); }, 600);
  }
  const toastArea = document.getElementById('toast-celebracao-area');
  if(toastArea && marcandoAgora && todasFeitas){
    toastArea.innerHTML = '<div class="toast-celebracao">✨ Exercício concluído! Mandou bem.</div>';
    setTimeout(function(){ if(toastArea) toastArea.innerHTML = ''; }, 2500);
  }
}

function concluirSerieBiset(subId, serieIdx, ehUltimoDoPar, segundosDescanso){
  if(!seriesConcluidas[subId]) return;
  seriesConcluidas[subId][serieIdx] = !seriesConcluidas[subId][serieIdx];
  const marcandoAgora = seriesConcluidas[subId][serieIdx];
  // Só dispara o descanso de verdade se for o segundo exercício do par (o primeiro não descansa)
  if(marcandoAgora && ehUltimoDoPar){
    iniciarDescanso(subId, segundosDescanso);
  }
  salvarProgressoNoSupabase(NOME_ALUNA_LOGADA);
  openDetail('dia', detailDiaAtual);
  const pill = document.getElementById('serie-pill-' + subId + '-' + serieIdx);
  if(pill && marcandoAgora){
    pill.classList.add('pulse');
    setTimeout(function(){ if(pill) pill.classList.remove('pulse'); }, 600);
  }
}

let cronometroDescansoAtivo = null; // { idx, restante, total, pausado }

let tabataAtivo = null;
let tabataInterval = null;

function iniciarTabata(diaTabata){
  if(tabataInterval) clearInterval(tabataInterval);
  tabataAtivo = { dados: diaTabata, blocoIdx: 0, cicloAtual: 1, fase: 'trabalho', restante: diaTabata.trabalhoSeg, pausado: false };

  abrirCronometroFullscreen();
  atualizarTelaTabata();

  tabataInterval = setInterval(function(){
    if(!tabataAtivo || tabataAtivo.pausado) return;
    tabataAtivo.restante--;

    if(tabataAtivo.restante > 0){
      atualizarTelaTabata();
      return;
    }

    // Acabou a fase atual, decide o que vem a seguir
    const d = tabataAtivo.dados;
    if(tabataAtivo.fase === 'trabalho'){
      tabataAtivo.fase = 'descanso';
      tabataAtivo.restante = d.descansoSeg;
    } else {
      if(tabataAtivo.cicloAtual < d.ciclos){
        tabataAtivo.cicloAtual++;
        tabataAtivo.fase = 'trabalho';
        tabataAtivo.restante = d.trabalhoSeg;
      } else if(tabataAtivo.blocoIdx < d.blocos.length - 1){
        tabataAtivo.blocoIdx++;
        tabataAtivo.cicloAtual = 1;
        tabataAtivo.fase = 'trabalho';
        tabataAtivo.restante = d.trabalhoSeg;
      } else {
        // Tabata inteiro concluído
        clearInterval(tabataInterval);
        tabataInterval = null;
        const label = document.getElementById('cronometro-fullscreen-label');
        const numero = document.getElementById('cronometro-fullscreen-numero');
        if(label) label.textContent = 'Tabata concluído!';
        if(numero) numero.textContent = '🎉';
        setTimeout(function(){ fecharCronometroFullscreen(); tabataAtivo = null; }, 2500);
        return;
      }
    }
    atualizarTelaTabata();
  }, 1000);
}

function atualizarTelaTabata(){
  if(!tabataAtivo) return;
  const d = tabataAtivo.dados;
  const nomeExercicio = d.blocos[tabataAtivo.blocoIdx].nome;
  const label = document.getElementById('cronometro-fullscreen-label');
  const numero = document.getElementById('cronometro-fullscreen-numero');
  const ring = document.getElementById('cronometro-fullscreen-ring');

  if(label){
    label.textContent = (tabataAtivo.fase === 'trabalho' ? 'Trabalho' : 'Descanso') + ' · ' + nomeExercicio + ' · Ciclo ' + tabataAtivo.cicloAtual + '/' + d.ciclos;
    label.style.color = tabataAtivo.fase === 'trabalho' ? 'var(--success)' : 'var(--text-faint)';
  }
  if(numero) numero.textContent = tabataAtivo.restante + 's';
  if(ring){
    const total = tabataAtivo.fase === 'trabalho' ? d.trabalhoSeg : d.descansoSeg;
    const proporcao = tabataAtivo.restante / total;
    ring.style.strokeDashoffset = String(616 - (616 * proporcao));
  }
}

function iniciarDescanso(idx, segundosTotais){
  if(descansoIntervals[idx]) clearInterval(descansoIntervals[idx]);
  cronometroDescansoAtivo = { idx: idx, restante: segundosTotais, total: segundosTotais, pausado: false };

  const display = document.getElementById('descanso-display-' + idx);
  if(display){ display.style.color = 'var(--gold-soft)'; display.textContent = segundosTotais + 's'; }

  abrirCronometroFullscreen();

  descansoIntervals[idx] = setInterval(function(){
    if(!cronometroDescansoAtivo || cronometroDescansoAtivo.pausado) return;
    cronometroDescansoAtivo.restante--;
    const restante = cronometroDescansoAtivo.restante;

    const elInline = document.getElementById('descanso-display-' + idx);
    const elFull = document.getElementById('cronometro-fullscreen-numero');
    const elPill = document.getElementById('cronometro-pill-numero');
    const ring = document.getElementById('cronometro-fullscreen-ring');

    if(restante > 0){
      if(elInline) elInline.textContent = restante + 's';
      if(elFull) elFull.textContent = restante + 's';
      if(elPill) elPill.textContent = restante + 's';
      if(ring){
        const proporcao = restante / cronometroDescansoAtivo.total;
        ring.setAttribute('stroke-dashoffset', String(616 * (1 - proporcao)));
      }
    } else {
      clearInterval(descansoIntervals[idx]);
      delete descansoIntervals[idx];
      if(elInline){ elInline.style.color = '#E2A33D'; elInline.textContent = '🔔 Vai! Próxima série'; }
      if(elFull) elFull.textContent = '🔔 Vai!';
      fecharCronometroFullscreen();
      cronometroDescansoAtivo = null;
    }
  }, 1000);
}

function abrirCronometroFullscreen(){
  if(!cronometroDescansoAtivo && !tabataAtivo) return;
  document.getElementById('cronometro-pill-minimizado').style.display = 'none';
  const overlay = document.getElementById('cronometro-fullscreen-overlay');
  overlay.style.display = 'flex';
  if(cronometroDescansoAtivo) document.getElementById('cronometro-fullscreen-numero').textContent = cronometroDescansoAtivo.restante + 's';
  document.getElementById('cronometro-fullscreen-btn').textContent = 'Pausar';
}

function minimizarCronometro(){
  document.getElementById('cronometro-fullscreen-overlay').style.display = 'none';
  if(!cronometroDescansoAtivo) return;
  const pill = document.getElementById('cronometro-pill-minimizado');
  pill.style.display = 'flex';
  document.getElementById('cronometro-pill-numero').textContent = cronometroDescansoAtivo.restante + 's';
}

function expandirCronometro(){
  abrirCronometroFullscreen();
}

function fecharCronometroFullscreen(){
  document.getElementById('cronometro-fullscreen-overlay').style.display = 'none';
  document.getElementById('cronometro-pill-minimizado').style.display = 'none';
}

function pausarOuRetomarCronometro(){
  if(tabataAtivo){
    tabataAtivo.pausado = !tabataAtivo.pausado;
    document.getElementById('cronometro-fullscreen-btn').textContent = tabataAtivo.pausado ? 'Retomar' : 'Pausar';
    return;
  }
  if(!cronometroDescansoAtivo) return;
  cronometroDescansoAtivo.pausado = !cronometroDescansoAtivo.pausado;
  document.getElementById('cronometro-fullscreen-btn').textContent = cronometroDescansoAtivo.pausado ? 'Retomar' : 'Pausar';
}

function pararCronometroFullscreen(){
  if(tabataAtivo){
    if(tabataInterval) clearInterval(tabataInterval);
    tabataInterval = null;
    tabataAtivo = null;
    fecharCronometroFullscreen();
    return;
  }
  if(cronometroDescansoAtivo && descansoIntervals[cronometroDescansoAtivo.idx]){
    clearInterval(descansoIntervals[cronometroDescansoAtivo.idx]);
    delete descansoIntervals[cronometroDescansoAtivo.idx];
  }
  const idx = cronometroDescansoAtivo ? cronometroDescansoAtivo.idx : null;
  cronometroDescansoAtivo = null;
  fecharCronometroFullscreen();
  if(idx !== null){
    const elInline = document.getElementById('descanso-display-' + idx);
    if(elInline){ elInline.style.color = 'var(--gold-soft)'; elInline.textContent = 'Descanso encerrado'; }
  }
}

const PALETA_IDENTIDADE = [
  { nome: 'Dourado Clássico', c1: '#F0D9A0', c2: '#E8C58A', c3: '#4C3E25' },
  { nome: 'Ouro Rosé', c1: '#F0D9C8', c2: '#C9967A', c3: '#8A5A42' },
  { nome: 'Champagne', c1: '#F5EBD8', c2: '#D9C08A', c3: '#9C8455' },
  { nome: 'Ouro Antigo', c1: '#E8D9A0', c2: '#B8942F', c3: '#6E5518' }
];

function corIdentidadeAluna(nome){
  let hash = 0;
  for(let i = 0; i < nome.length; i++){ hash = (hash * 31 + nome.charCodeAt(i)) % 1000; }
  return PALETA_IDENTIDADE[hash % PALETA_IDENTIDADE.length];
}

function aplicarIdentidadeVisual(nome){
  const cor = corIdentidadeAluna(nome);
  const avatar = document.getElementById('avatar-launcher');
  const primeiroNome = (nome || '').split(' ')[0];
  if(avatar){
    avatar.style.background = 'radial-gradient(circle at 30% 25%, ' + cor.c1 + ', ' + cor.c2 + ' 55%, ' + cor.c3 + ' 100%)';
    avatar.style.boxShadow = '0 8px 30px ' + cor.c2 + '59';
    const letraSpan = avatar.querySelector('span');
    if(letraSpan) letraSpan.textContent = primeiroNome.charAt(0).toUpperCase();
  }
  const nomeProfileEl = document.getElementById('nome-aluna-launcher-centro');
  const alunaObjIdentidade = alunasPersonal.find(function(a){ return a.nome === nome; });
  const avatarComposicao = document.getElementById('avatar-composicao');
  if(avatarComposicao){
    const spanComp = avatarComposicao.querySelector('span');
    if(spanComp) spanComp.textContent = '%';
  }
  if(nomeProfileEl) nomeProfileEl.textContent = primeiroNome;
  const tituloHomeEl = document.querySelector('[data-view="home"] .page-title');
  if(tituloHomeEl) tituloHomeEl.textContent = 'Olá, ' + primeiroNome;
}
aplicarIdentidadeVisual(NOME_ALUNA_LOGADA);

function alternarCronometro(){
  const btn = document.getElementById('cronometro-btn');
  if(!cronometroRodando){
    cronometroRodando = true;
    btn.textContent = 'Pausar';
    cronometroInterval = setInterval(function(){
      cronometroSegundos++;
      const display = document.getElementById('cronometro-display');
      if(display){
        const min = String(Math.floor(cronometroSegundos / 60)).padStart(2, '0');
        const seg = String(cronometroSegundos % 60).padStart(2, '0');
        display.textContent = min + ':' + seg;
      }
    }, 1000);
  } else {
    cronometroRodando = false;
    btn.textContent = 'Retomar';
    clearInterval(cronometroInterval);
  }
}

function calcularDescansoPorReps(reps){
  if(reps == null || isNaN(reps)) return null;
  if(reps >= 11) return '50-60s';
  if(reps >= 9) return '90s';
  return '120-180s'; // 6-8 reps, volume baixo / período de choque
}

function descansoParaSegundos(faixaTexto){
  if(faixaTexto === '50-60s') return 55;
  if(faixaTexto === '90s') return 90;
  if(faixaTexto === '120-180s') return 150;
  return 60;
}

function getEmbedUrl(url){
  if(!url) return '';
  const m1 = url.match(/youtu\.be\/([A-Za-z0-9_-]+)/);
  const m2 = url.match(/[?&]v=([A-Za-z0-9_-]+)/);
  const id = m1 ? m1[1] : (m2 ? m2[1] : '');
  return id ? 'https://www.youtube.com/embed/' + id : '';
}

function ytFallback(url){
  return url ? '<a class="yt-fallback" href="' + url + '" target="_blank" rel="noopener"><i class="ti ti-external-link" style="font-size:12px;"></i>O player não carregou? Abrir no YouTube</a>' : '';
}

let currentCursoMeta = null;

function abrirCurso(c){
  currentCursoMeta = c;
  renderPlaylist();
}

function renderPlaylist(){
  const el = document.getElementById('detail-content');
  const c = currentCursoMeta;
  let itens = '';
  c.aulas.forEach(function(a, i){
    itens += '<div class="list-item" style="cursor:pointer;" onclick="playAula(' + i + ')"><span>' + (i+1) + '. ' + a.titulo + '</span><i class="ti ti-player-play" style="font-size:13px;color:var(--gold-soft);"></i></div>';
  });
  el.innerHTML =
    '<p class="page-sub" style="margin-top:14px;">' + c.cat + ' · Grátis</p>' +
    '<h1 class="page-title" style="margin-top:0;">' + c.n + '</h1>' +
    '<div class="info-box"><p class="txt">' + c.desc + '</p></div>' +
    '<p class="section-label">Aulas (' + c.aulas.length + ')</p>' +
    itens;
}

function playAula(i){
  const a = currentCursoMeta.aulas[i];
  const embed = getEmbedUrl(a.video);
  const el = document.getElementById('detail-content');
  el.innerHTML =
    '<div class="local-back" style="margin-top:14px;" onclick="renderPlaylist()"><i class="ti ti-arrow-left"></i><span>Lista de aulas</span></div>' +
    (embed
      ? '<div class="video-block"><iframe src="' + embed + '" title="' + a.titulo + '" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></div>' + ytFallback(a.video)
      : '<div class="video-block"><i class="ti ti-player-play"></i></div>') +
    '<h1 class="page-title" style="margin-top:0;">' + a.titulo + '</h1>';
}

function calcularEfeitoCardio(minutosSemana){
  const m = minutosSemana || 0;
  const ajusteDefinicao = Math.min(25, Math.round(m / 8));
  const ajusteGordura = -Math.min(20, Math.round(m / 10));
  let ajusteHipertrofia;
  if(m === 0) ajusteHipertrofia = 0;
  else if(m <= 150) ajusteHipertrofia = 3; // zona ideal (bate com recomendação da OMS), sem interferência relevante
  else if(m <= 250) ajusteHipertrofia = 0; // ainda seguro, neutro
  else ajusteHipertrofia = -5; // volume muito alto, efeito de interferência começa a aparecer na literatura
  return { ajusteDefinicao: ajusteDefinicao, ajusteGordura: ajusteGordura, ajusteHipertrofia: ajusteHipertrofia, minutosSemana: m };
}

function calcularEfeitoSono(horasMediasNoite){
  if(horasMediasNoite == null || isNaN(horasMediasNoite)) return { ajusteRecuperacao: 0 };
  if(horasMediasNoite >= 7 && horasMediasNoite <= 9) return { ajusteRecuperacao: 10 }; // faixa ideal (recomendação geral pra adultos)
  if(horasMediasNoite > 9) return { ajusteRecuperacao: 5 }; // acima do ideal, ainda positivo
  if(horasMediasNoite >= 6) return { ajusteRecuperacao: 0 }; // zona neutra
  return { ajusteRecuperacao: -12 }; // abaixo de 6h cronicamente prejudica recuperação (literatura: até -20% síntese proteica)
}

function calcularNutricaoSemana(fugas, tipos, boaConstanciaSemana, dosesAlcool, alcoolFrequente, aguaLitros, metaAguaLitros, cardioMinutos, horasSono){
  const respostaNutricional = fugas === 0 ? 95 : Math.max(30, 90 - fugas * 12);
  let ajusteHipertrofia = 0;
  let ajusteGordura = 0;
  let ajusteRetencao = 0;
  if(tipos.indexOf('doce') !== -1){ ajusteHipertrofia -= 8; ajusteGordura += 10; }
  if(tipos.indexOf('mais') !== -1){ ajusteHipertrofia += boaConstanciaSemana ? 5 : 1; ajusteGordura += 1; }
  if(tipos.indexOf('menos') !== -1){ ajusteHipertrofia -= 3; ajusteGordura -= 2; }
  const doses = dosesAlcool || 0;
  if(doses > 0){
    const multiplicador = alcoolFrequente ? 1.6 : 1; // vira padrão repetido pesa mais que um episódio isolado
    ajusteHipertrofia -= Math.round(doses * 2 * multiplicador);
    ajusteGordura += Math.round(doses * 2 * multiplicador);
    ajusteRetencao += Math.round(doses * 3 * multiplicador);
  }
  let pctHidratacao = null;
  if(aguaLitros != null && metaAguaLitros){
    pctHidratacao = Math.round((aguaLitros / metaAguaLitros) * 100);
    if(pctHidratacao < 70){
      ajusteRetencao += Math.round((70 - pctHidratacao) / 2); // hipohidratação crônica → retenção paradoxal
    }
  }
  const efeitoCardio = calcularEfeitoCardio(cardioMinutos);
  ajusteHipertrofia += efeitoCardio.ajusteHipertrofia;
  ajusteGordura += efeitoCardio.ajusteGordura;
  const efeitoSono = calcularEfeitoSono(horasSono);
  return { respostaNutricional: respostaNutricional, ajusteHipertrofia: ajusteHipertrofia, ajusteGordura: ajusteGordura, ajusteRetencao: ajusteRetencao, dosesAlcool: doses, pctHidratacao: pctHidratacao, ajusteDefinicao: efeitoCardio.ajusteDefinicao, cardioMinutos: efeitoCardio.minutosSemana, ajusteRecuperacaoSono: efeitoSono.ajusteRecuperacao, horasSono: horasSono };
}

function calcularNutricaoStats(nome){
  const prog = getProgressoAluna(nome);
  const semanas = Object.keys(prog.nutricao || {});
  if(semanas.length === 0) return { temDados: false };
  let somaResposta = 0, somaAjusteHipertrofia = 0, somaAjusteGordura = 0, somaAjusteRetencao = 0, somaAjusteDefinicao = 0, somaAjusteRecuperacaoSono = 0, semanasComAlcool = 0, semanasComCardio = 0, semanasComSono = 0;
  semanas.forEach(function(s){
    const n = prog.nutricao[s];
    somaResposta += n.resultado.respostaNutricional;
    somaAjusteHipertrofia += n.resultado.ajusteHipertrofia;
    somaAjusteGordura += n.resultado.ajusteGordura;
    somaAjusteRetencao += (n.resultado.ajusteRetencao || 0);
    somaAjusteDefinicao += (n.resultado.ajusteDefinicao || 0);
    if(n.resultado.horasSono != null && !isNaN(n.resultado.horasSono)){
      somaAjusteRecuperacaoSono += (n.resultado.ajusteRecuperacaoSono || 0);
      semanasComSono++;
    }
    if(n.resultado.dosesAlcool > 0) semanasComAlcool++;
    if(n.resultado.cardioMinutos > 0) semanasComCardio++;
  });
  const respostaMedia = Math.round(somaResposta / semanas.length);
  const potencialHipertrofia = Math.max(20, Math.min(95, 60 + somaAjusteHipertrofia));
  const potencialGanhoGordura = Math.max(10, Math.min(95, 25 + somaAjusteGordura));
  const retencaoHidricaEstimada = Math.max(0, Math.min(100, somaAjusteRetencao));
  const mediaAjusteRecuperacaoSono = semanasComSono > 0 ? Math.round(somaAjusteRecuperacaoSono / semanasComSono) : 0;
  return { temDados: true, respostaNutricional: respostaMedia, potencialHipertrofia: potencialHipertrofia, potencialGanhoGordura: potencialGanhoGordura, retencaoHidricaEstimada: retencaoHidricaEstimada, ajusteGorduraAcumulado: somaAjusteGordura, somaAjusteDefinicao: somaAjusteDefinicao, ajusteRecuperacaoSono: mediaAjusteRecuperacaoSono, semanasRespondidas: semanas.length, semanasComAlcool: semanasComAlcool, semanasComCardio: semanasComCardio, semanasComSono: semanasComSono };
}

function calcularPotencialDefinicao(nome){
  const stats = calcularEstatisticasAluna(nome);
  const nutriStats = calcularNutricaoStats(nome);
  if(!stats.temDados || !nutriStats.temDados) return null;
  const pctConstancia = Math.round((stats.totalConcluidos / stats.totalPlanejado) * 100);
  return Math.max(10, Math.min(95, Math.round(30 + 0.3 * pctConstancia + nutriStats.somaAjusteDefinicao)));
}

function explicacaoIndicador(nome, valor){
  const textos = {
    'Constância': valor >= 80 ? 'Excelente constância, é a base de tudo, continue assim.' : valor >= 60 ? 'Boa constância. Tente não deixar passar mais de 1 treino por semana.' : 'Tente manter mais dias de treino por semana, é o que mais pesa em todo o sistema.',
    'Capacidade de progressão': 'Não esqueça de anotar carga e repetições certinho na 1ª série de cada exercício, é isso que faz esse número refletir sua evolução real.',
    'Capacidade de recuperação': valor >= 70 ? 'Sua recuperação está indo bem, continue priorizando sono e hidratação.' : 'Priorize sono (7-9h por noite) e hidratação essa semana, isso ajuda diretamente esse número a subir.',
    'Potencial de hipertrofia': valor >= 60 ? 'Seguir a dieta está ajudando esse potencial a se destravar.' : 'Manter o combinado da dieta, principalmente evitando doces e frituras fora do prescrito, ajuda a destravar esse potencial.',
    'Resposta nutricional': valor >= 70 ? 'Ótima adesão à dieta, continue assim.' : 'Quanto mais você seguir o combinado, mais esse número sobe, pequenos ajustes já fazem diferença.',
    'Potencial de ganho de gordura': valor <= 35 ? 'Você está mantendo esse risco baixo, ótimo trabalho.' : 'Fique de olho nas fugas com doces e frituras, elas pesam mais nesse número que "comer um pouco mais" do prescrito.',
    'Potencial de definição física': valor >= 60 ? 'Sua constância e o cardio estão ajudando bastante esse número.' : 'Manter constância no treino e incluir cardio moderado (até 150min/semana) ajuda a subir esse indicador, sem prejudicar sua hipertrofia.'
  };
  return textos[nome] || '';
}

let semanaNutricaoPendente = null;

function renderPerguntaNutricao(semanaFechada){
  semanaNutricaoPendente = semanaFechada;
  return '<div class="info-box" id="area-nutricao" style="margin-top:10px;">' +
    '<p class="lbl">Check-in nutricional da semana</p>' +
    '<p class="txt">Você seguiu a dieta essa semana? Sem julgamento, só queremos calibrar direitinho, quantas vezes você fugiu do planejado?</p>' +
    '<div class="form-group"><input class="form-input" id="nutri-fugas" type="number" placeholder="Ex: 0"></div>' +
    '<div class="form-group"><label class="form-label">Se fugiu, foi principalmente sobre o quê? (marque quantas se aplicarem)</label>' +
      '<div style="display:flex;flex-direction:column;gap:6px;font-size:12px;">' +
        '<label><input type="checkbox" id="nutri-mais"> Comi mais do que o prescrito</label>' +
        '<label><input type="checkbox" id="nutri-menos"> Comi menos do que o prescrito</label>' +
        '<label><input type="checkbox" id="nutri-doce"> Doces, frituras ou lanches fora do prescrito</label>' +
      '</div>' +
    '</div>' +
    '<p class="txt" style="margin-top:10px;">E sobre álcool essa semana? Pode ser sincera, sem julgamento, é só pra manter nosso perfil o mais assertivo possível.</p>' +
    '<div class="form-group"><label class="form-label">Taças de vinho (150ml cada)</label><input class="form-input" id="nutri-alc-taca" type="number" placeholder="0"></div>' +
    '<div class="form-group"><label class="form-label">Garrafas de vinho (750ml cada)</label><input class="form-input" id="nutri-alc-garrafa" type="number" placeholder="0"></div>' +
    '<div class="form-group"><label class="form-label">Litros de vinho</label><input class="form-input" id="nutri-alc-litro" type="number" placeholder="0"></div>' +
    '<div class="form-group"><label class="form-label">Copos de cerveja (chope, 300ml cada)</label><input class="form-input" id="nutri-alc-copo-cerveja" type="number" placeholder="0"></div>' +
    '<div class="form-group"><label class="form-label">Latas de cerveja (350ml cada)</label><input class="form-input" id="nutri-alc-lata-cerveja" type="number" placeholder="0"></div>' +
    '<p class="txt" style="margin-top:10px;">Por último, em média, quantos litros de água você bebeu por dia essa semana?</p>' +
    '<div class="form-group"><input class="form-input" id="nutri-agua" type="number" step="0.1" placeholder="Ex: 2.5"></div>' +
    '<p class="txt" style="margin-top:10px;">E cardio essa semana, fez algum?</p>' +
    '<div class="form-group"><label class="form-label">Quantos dias de cardio?</label><input class="form-input" id="nutri-cardio-dias" type="number" placeholder="Ex: 2"></div>' +
    '<div class="form-group"><label class="form-label">Tempo total, em minutos, somando todos os dias</label><input class="form-input" id="nutri-cardio-minutos" type="number" placeholder="Ex: 60"></div>' +
    '<p class="txt" style="margin-top:10px;">Por último, em média, quantas horas você dormiu por noite essa semana?</p>' +
    '<div class="form-group"><input class="form-input" id="nutri-sono" type="number" step="0.5" placeholder="Ex: 7"></div>' +
    '<button class="btn-gold" style="background:var(--card-2);color:var(--gold-soft);border:1px solid var(--border);" onclick="registrarNutricaoSemana()">Enviar</button>' +
    '</div>';
}

function converterAlcoolParaDoses(){
  const taca = parseFloat(document.getElementById('nutri-alc-taca').value) || 0;
  const garrafa = parseFloat(document.getElementById('nutri-alc-garrafa').value) || 0;
  const litro = parseFloat(document.getElementById('nutri-alc-litro').value) || 0;
  const copoCerveja = parseFloat(document.getElementById('nutri-alc-copo-cerveja').value) || 0;
  const lataCerveja = parseFloat(document.getElementById('nutri-alc-lata-cerveja').value) || 0;
  // 1 dose ≈ 150ml de vinho ou ~330ml de cerveja
  const doses = (taca * 1) + (garrafa * 5) + (litro * 6.7) + (copoCerveja * 1) + (lataCerveja * 1);
  return Math.round(doses * 10) / 10;
}

function extrairPesoKg(pesoTexto){
  if(!pesoTexto) return 65;
  const m = String(pesoTexto).match(/(\d+(?:[.,]\d+)?)/);
  return m ? parseFloat(m[1].replace(',', '.')) : 65;
}

function calcularMetaAguaLitros(pesoTexto){
  const peso = extrairPesoKg(pesoTexto);
  return Math.round(peso * 32.5) / 1000;
}

function registrarNutricaoSemana(){
  const alunaAtual = alunasPersonal.find(function(a){ return a.nome === NOME_ALUNA_LOGADA; });
  if(!alunaAtual || semanaNutricaoPendente === null) return;
  const prog = getProgressoAluna(alunaAtual.nome);
  const fugas = parseInt(document.getElementById('nutri-fugas').value, 10) || 0;
  const tipos = [];
  if(document.getElementById('nutri-mais').checked) tipos.push('mais');
  if(document.getElementById('nutri-menos').checked) tipos.push('menos');
  if(document.getElementById('nutri-doce').checked) tipos.push('doce');
  const dosesAlcool = converterAlcoolParaDoses();
  const aguaLitros = parseFloat(document.getElementById('nutri-agua').value);
  const metaAgua = calcularMetaAguaLitros(alunaAtual.peso);
  const cardioMinutos = parseFloat(document.getElementById('nutri-cardio-minutos').value) || 0;
  const horasSonoInput = parseFloat(document.getElementById('nutri-sono').value);
  const horasSono = isNaN(horasSonoInput) ? null : horasSonoInput;

  const totalDias = totalDiasDeTreino();
  const concluidosNaSemana = (prog.diasConcluidos[semanaNutricaoPendente] || []).length;
  const boaConstancia = concluidosNaSemana >= totalDias;

  const semanasAnteriores = [semanaNutricaoPendente - 1, semanaNutricaoPendente - 2].filter(function(s){ return prog.nutricao && prog.nutricao[s]; });
  const semanasComAlcoolRecente = semanasAnteriores.filter(function(s){ return prog.nutricao[s].resultado.dosesAlcool > 0; }).length;
  const alcoolFrequente = dosesAlcool > 0 && semanasComAlcoolRecente >= 1;

  const resultado = calcularNutricaoSemana(fugas, tipos, boaConstancia, dosesAlcool, alcoolFrequente, isNaN(aguaLitros) ? null : aguaLitros, metaAgua, cardioMinutos, horasSono);
  if(!prog.nutricao) prog.nutricao = {};
  prog.nutricao[semanaNutricaoPendente] = { fugas: fugas, tipos: tipos, resultado: resultado };
  semanaNutricaoPendente = null;
  salvarProgressoNoSupabase(NOME_ALUNA_LOGADA);

  const el = document.getElementById('area-nutricao') || document.getElementById('nutri-confirmacao');
  if(el) el.innerHTML = '<p class="txt">Obrigada! Isso já ajustou sua Resposta Nutricional, Potencial de Hipertrofia, Potencial de Ganho de Gordura e Retenção Hídrica no DNA MUSA.</p>';
}


function calcularProbabilidadeSucesso(nome){
  const stats = calcularEstatisticasAluna(nome);
  if(!stats.temDados) return null;
  const alunaObj = alunasPersonal.find(function(a){ return a.nome === nome; });
  const ajusteIdade = ajusteRecuperacaoPorIdade(alunaObj ? alunaObj.idade : null);
  const pctConstancia = Math.round((stats.totalConcluidos / stats.totalPlanejado) * 100);
  const nutriStatsRec = calcularNutricaoStats(nome);
  const ajusteSono = nutriStatsRec.temDados ? nutriStatsRec.ajusteRecuperacaoSono : 0;
  const capRecuperacao = Math.max(20, Math.min(95, (stats.progressoes >= 2 ? 68 : 60) + ajusteIdade.ajuste + ajusteSono));
  const capProgressao = Math.min(90, 55 + stats.progressoes * 10);
  const nutriStats = calcularNutricaoStats(nome);
  const potHipertrofia = nutriStats.temDados ? nutriStats.potencialHipertrofia : 50;
  const respNutricional = nutriStats.temDados ? nutriStats.respostaNutricional : 50;
  const potGordura = nutriStats.temDados ? nutriStats.potencialGanhoGordura : 30;

  const bruto =
    0.30 * pctConstancia +
    0.20 * capProgressao +
    0.15 * capRecuperacao +
    0.15 * potHipertrofia +
    0.10 * respNutricional +
    0.10 * (100 - potGordura);

  const score = Math.max(10, Math.min(97, Math.round(bruto)));
  registrarHistoricoDnaScore(nome, score);
  return score;
}

// Guarda um ponto por semana, sem duplicar, pra poder plotar a evolução em "Meu Progresso"
function registrarHistoricoDnaScore(nome, score){
  const prog = getProgressoAluna(nome);
  if(!prog.dnaScoreHistorico) prog.dnaScoreHistorico = [];
  const jaTemEssaSemana = prog.dnaScoreHistorico.find(function(r){ return r.semana === prog.semana; });
  if(jaTemEssaSemana){
    jaTemEssaSemana.score = score; // atualiza, caso o score tenha mudado dentro da mesma semana
  } else {
    prog.dnaScoreHistorico.push({ semana: prog.semana, score: score, data: new Date().toISOString().slice(0,10) });
  }
}

/* ===== REGRAS DE COMBINAÇÃO (BI-SET / TRI-SET) ===== */

const UNILATERAIS_BLOQUEADOS = ['AFUNDO', 'BÚLGARO', 'LEG UNILATERAL'];
const PARES_PROIBIDOS = [
  ['CADEIRA FLEXORA', 'CAMA FLEXORA'],
  ['CADEIRA FLEXORA', 'MESA FLEXORA']
];

function validarCombinacaoBiset(nomeEx1, nomeEx2){
  const n1 = nomeEx1.toUpperCase();
  const n2 = nomeEx2.toUpperCase();

  for(const par of PARES_PROIBIDOS){
    const bate = (n1.indexOf(par[0]) !== -1 && n2.indexOf(par[1]) !== -1) || (n1.indexOf(par[1]) !== -1 && n2.indexOf(par[0]) !== -1);
    if(bate) return { valido: false, motivo: 'Essa combinação sobrecarrega demais o mesmo padrão de movimento sem ganho real.' };
  }

  const ehUnilateralBloqueado = function(n){ return UNILATERAIS_BLOQUEADOS.some(function(u){ return n.indexOf(u) !== -1 && n.indexOf('ALTERNAD') === -1; }); };
  const bloq1 = ehUnilateralBloqueado(n1);
  const bloq2 = ehUnilateralBloqueado(n2);
  if(bloq1 || bloq2){
    return { valido: false, motivo: 'Não combinamos um exercício bilateral/multiarticular com um unilateral "bloqueado" (uma perna, depois a outra), desequilibra o ritmo do bi-set. Use a versão alternada, se existir.' };
  }

  return { valido: true, motivo: '' };
}

/* ===== ALTERNATIVA DE EMERGÊNCIA (equipamento ocupado) ===== */

const ALTERNATIVAS_HALTERES = {
  'REMADA': ['Remada curvada com halteres', 'Remada aberta com halteres', 'Remada curvada com barra', 'Remada aberta com barra'],
  'LEG PRESS': ['Agachamento com halteres'],
  'CADEIRA EXTENSORA': [],
  'CADEIRA FLEXORA': ['Stiff com halteres'],
  'CAMA FLEXORA': ['Stiff com halteres'],
  'AGACHAMENTO': ['Agachamento com halteres'],
  'ELEVAÇÃO PÉLVICA': ['Stiff com halteres']
};

function sugerirAlternativaEmergencia(nomeExercicio){
  const nomeUpper = nomeExercicio.toUpperCase();
  let opcoesHalteres = [];
  Object.keys(ALTERNATIVAS_HALTERES).forEach(function(chave){
    if(nomeUpper.indexOf(chave) !== -1){ opcoesHalteres = opcoesHalteres.concat(ALTERNATIVAS_HALTERES[chave]); }
  });
  opcoesHalteres = opcoesHalteres.filter(function(nome, i){ return opcoesHalteres.indexOf(nome) === i; });
  const disponiveisNoBanco = opcoesHalteres.filter(function(nome){ return buscarExercicioNoBanco(nome); });
  return {
    nivel1_halteres: disponiveisNoBanco,
    nivel2_backup: 'Verifique se há um backup pré-aprovado cadastrado para este exercício.',
    nivel3_pular: 'Pule para o próximo exercício da sequência e volte a este quando o equipamento desocupar.'
  };
}

/* ===== PORTÃO DE TÉCNICA ===== */

function precisaAprovacaoTecnica(a, nomeExercicio){
  if(!a.tecnicaAprovada) a.tecnicaAprovada = {};
  const status = a.tecnicaAprovada[nomeExercicio];
  if(status === 'aprovado') return false;
  if(status === 'pendente') return true; // já foi solicitado (inclusive pela exceção de desconforto), sempre mostra até resolver
  if(a.nivel === 'Iniciante') return status !== 'aprovado';
  // Intermediária/avançada: checagem periódica, não obrigatória, só sinaliza se nunca foi verificado
  return status === undefined ? 'periodica' : false;
}

function solicitarVideoTecnica(nomeAluna, nomeExercicio){
  const a = alunasPersonal.find(function(x){ return x.nome === nomeAluna; });
  if(!a) return;
  if(!a.tecnicaAprovada) a.tecnicaAprovada = {};
  a.tecnicaAprovada[nomeExercicio] = 'pendente';
  salvarPerfilAlunaNoSupabase(nomeAluna);
}

function aprovarTecnica(nomeAluna, nomeExercicio){
  const a = alunasPersonal.find(function(x){ return x.nome === nomeAluna; });
  if(!a) return;
  if(!a.tecnicaAprovada) a.tecnicaAprovada = {};
  a.tecnicaAprovada[nomeExercicio] = 'aprovado';
  salvarPerfilAlunaNoSupabase(nomeAluna);
  const i = alunasPersonal.indexOf(a);
  openAlunaDetail(i);
}

/* ===== PERIODIZAÇÃO (Base x Choque), a partir de intermediária ===== */

function renderExerciciosEstagnados(a){
  const estagnados = detectarExerciciosEstagnados(a);
  if(estagnados.length === 0) return '';
  let html = '<p class="section-label" style="margin-top:22px;">Exercícios estagnados</p>' +
    '<p class="page-sub" style="margin-top:-4px;">Mesma carga por 3 sessões seguidas, sem progressão. Critério objetivo pra considerar trocar.</p>';
  estagnados.forEach(function(e){
    html += '<div class="list-item"><span>' + e.nome + '</span><span class="tag">' + e.carga + 'kg há ' + e.sessoes + ' sessões</span></div>';
  });
  return html;
}

function detectarExerciciosEstagnados(a){
  const prog = getProgressoAluna(a.nome);
  const estagnados = [];
  Object.keys(prog.historico || {}).forEach(function(nomeEx){
    const registros = prog.historico[nomeEx];
    if(registros.length < 3) return;
    const ultimos3 = registros.slice(-3);
    // O que importa é a carga que ela de fato registrou, não o que o sistema sugeriu no meio do caminho
    const mesmaCarga = ultimos3.every(function(r){ return r.carga === ultimos3[0].carga; });
    if(mesmaCarga){
      estagnados.push({ nome: nomeEx, carga: ultimos3[0].carga, sessoes: ultimos3.length });
    }
  });
  return estagnados;
}

// ===== MOTOR 8: INTELIGÊNCIA ADAPTATIVA — IRT (uso interno, nunca exibido pra aluna) =====
// Responde: a estratégia atual continua sendo a melhor pra essa aluna?
function calcularIRT(a){
  const prog = getProgressoAluna(a.nome);
  const stats = calcularEstatisticasAluna(a.nome);
  const fadiga = avaliarSinaisDeFadiga(a);
  let pontos = 50; // ponto de partida neutro

  // Evolução de carga: cada progressão real puxa pra cima
  if(stats.temDados) pontos += Math.min(20, stats.progressoes * 4);

  // Aderência: constância puxa proporcionalmente
  if(stats.temDados){
    const pctConstancia = stats.totalConcluidos / stats.totalPlanejado;
    pontos += Math.round((pctConstancia - 0.5) * 40); // 100% constância = +20, 0% = -20
  }

  // Fadiga acumulada: penaliza
  if(fadiga.nivel === 'alto') pontos -= 25;
  else if(fadiga.nivel === 'moderado') pontos -= 10;

  // Regularidade de check-in nutricional, como proxy de engajamento geral
  const semanasComCheckinNutricao = Object.keys(prog.nutricao || {}).length;
  if(semanasComCheckinNutricao >= 3) pontos += 5;

  return Math.max(0, Math.min(100, Math.round(pontos)));
}

function gerarDiagnosticoInterno(a){
  const irt = calcularIRT(a);
  const estrategia = definirEstrategiaAtual(a);
  const classificacao = classificarAluna(a);
  const faseInfo = determinarFaseMacrociclo(a);

  const pontosFortes = [];
  const gargalos = [];
  if(classificacao.constanciaPct != null && classificacao.constanciaPct >= 70) pontosFortes.push('Boa constância (' + classificacao.constanciaPct + '%)');
  if(classificacao.fadiga.nivel === 'baixo') pontosFortes.push('Sem sinais de fadiga acumulada');
  if(classificacao.experienciaTecnica !== 'iniciante') pontosFortes.push('Já tem base técnica consolidada');
  if(classificacao.constanciaPct != null && classificacao.constanciaPct < 60) gargalos.push('Constância abaixo do ideal (' + classificacao.constanciaPct + '%)');
  if(classificacao.fadiga.nivel !== 'baixo') gargalos.push.apply(gargalos, classificacao.fadiga.motivos);

  return {
    irt: irt,
    estrategiaAtual: estrategia.nome,
    justificativaEstrategia: estrategia.motivo,
    faseAtual: faseInfo.fase,
    pontosFortes: pontosFortes,
    gargalos: gargalos,
    prioridadeMuscular: classificacao.grupoPrioritario,
    resumo: 'IRT ' + irt + '/100 · Estratégia: ' + estrategia.nome + ' · Fase: ' + faseInfo.fase
  };
}

function avaliarSinaisDeFadiga(a){
  const prog = getProgressoAluna(a.nome);
  const motivos = [];
  let pontos = 0;

  // Sinal 1: feedback pós-treino recente com desconforto alto ou intensidade sempre no limite
  const feedbacksRecentes = (prog.feedbackTreino || []).slice(-4);
  const comDesconfortoAlto = feedbacksRecentes.filter(function(f){ return f.desconforto && f.escalaDesconforto >= 6; }).length;
  if(comDesconfortoAlto >= 2){ motivos.push('Desconforto alto relatado em ' + comDesconfortoAlto + ' dos últimos treinos'); pontos++; }

  // Sinal 2: sono ruim nas semanas recentes
  const semanasNutricao = Object.keys(prog.nutricao || {}).map(Number).sort(function(x,y){ return y-x; }).slice(0, 3);
  const semSonoRuim = semanasNutricao.filter(function(s){
    const r = prog.nutricao[s] && prog.nutricao[s].resultado;
    return r && r.ajusteRecuperacaoSono != null && r.ajusteRecuperacaoSono < 0;
  }).length;
  if(semSonoRuim >= 2){ motivos.push('Sono insuficiente em ' + semSonoRuim + ' das últimas semanas'); pontos++; }

  // Sinal 3: histórico de carga mostrando reduções seguidas em mais de 1 exercício
  const exerciciosComQuedaConsecutiva = Object.keys(prog.historico || {}).filter(function(ex){
    const registros = prog.historico[ex].slice(-2);
    return registros.length === 2 && registros.every(function(r){ return r.sugestao && r.sugestao.texto.indexOf('edução') !== -1; });
  });
  if(exerciciosComQuedaConsecutiva.length >= 2){ motivos.push('Queda de carga em 2 sessões seguidas em ' + exerciciosComQuedaConsecutiva.length + ' exercícios'); pontos++; }

  // Sinal 4: baixa aderência recente (constância caindo)
  const semanasRecentes = Object.keys(prog.diasConcluidos || {}).map(Number).sort(function(x,y){ return y-x; }).slice(0, 2);
  const totalDias = totalDiasDeTreino();
  const semanasBaixaAderencia = semanasRecentes.filter(function(s){ return (prog.diasConcluidos[s] || []).length < totalDias * 0.6; }).length;
  if(semanasBaixaAderencia >= 2){ motivos.push('Baixa aderência nas últimas 2 semanas'); pontos++; }

  if(pontos >= 3) return { nivel: 'alto', motivos: motivos };
  if(pontos >= 1) return { nivel: 'moderado', motivos: motivos };
  return { nivel: 'baixo', motivos: [] };
}

// ===== MOTOR 7: ESTRATÉGIA — o cérebro central =====
// Antes de montar qualquer treino, responde: qual é a melhor estratégia pra essa aluna agora?
// Usa o que os Motores 2 (Classificação), 3 (Periodização) e a análise de fadiga/aderência já calculam.
// Regra nova: 3 semanas seguidas com constância abaixo de 40% é sinal real de risco de abandono —
// diferente de fadiga (que é sobre estar treinando mas cansada). Aqui ela simplesmente não está indo.
// ===== CENTRAL DE AVISOS =====
const templatesAvisos = {
  riscoAbandono: function(a){ return 'Oi ' + a.nome.split(' ')[0] + '! Notei que você deu uma pausa nos treinos essas últimas semanas. Reduzi um pouco o volume pra facilitar a volta — bora retomar juntas? 💛'; },
  avaliacaoAtrasada: function(a){ return 'Oi ' + a.nome.split(' ')[0] + '! Já faz um tempo desde sua última avaliação física. Vamos agendar uma nova pra acompanhar sua evolução direitinho?'; },
  planoVencendo: function(a){ return 'Oi ' + a.nome.split(' ')[0] + '! Seu plano está perto de vencer. Vamos conversar sobre a renovação?'; },
  planoVencido: function(a){ return 'Oi ' + a.nome.split(' ')[0] + '! Seu plano venceu. Sentimos sua falta — vamos renovar?'; },
  treinoProgredido: function(a){ return 'Oi ' + a.nome.split(' ')[0] + '! Seu treino acabou de ser atualizado no app, com evolução de carga/variedade. Já pode conferir! 💪'; },
  metaComunidade: function(pontos){ return '🎉 A comunidade bateu a meta de ' + pontos.toLocaleString('pt-BR') + ' pontos! Obrigada por fazer parte disso — em breve mais novidades.'; },
  aniversario: function(a){ return '🎂 Feliz aniversário, ' + a.nome.split(' ')[0] + '! Que seu novo ano seja repleto de saúde, força e conquistas. Muito feliz de fazer parte dessa jornada com você! 💛'; },
  diaDaMulher: function(a){ return 'Feliz Dia da Mulher, ' + a.nome.split(' ')[0] + '! Que você continue se superando e se cuidando cada dia mais. Você merece todo o reconhecimento. 💐'; },
  diaDasMaes: function(a){ return 'Feliz Dia das Mães, ' + a.nome.split(' ')[0] + '! Que esse dia seja tão especial quanto você é para quem te ama. 💐'; }
};

// ===== DATAS ESPECIAIS (aniversário, Dia da Mulher, Dia das Mães) =====
function alunasAniversarioHoje(){
  const hoje = new Date();
  const mesHoje = hoje.getMonth() + 1, diaHoje = hoje.getDate();
  return alunasPersonal.filter(function(a){
    if(a.status === 'lead' || !a.telefone || !a.dataNascimento) return false;
    const partes = a.dataNascimento.split('-'); // formato YYYY-MM-DD
    if(partes.length < 3) return false;
    return parseInt(partes[1], 10) === mesHoje && parseInt(partes[2], 10) === diaHoje;
  });
}

function segundoDomingoDeMaio(ano){
  const primeiro = new Date(ano, 4, 1); // maio = índice 4
  const diaSemanaPrimeiro = primeiro.getDay(); // 0 = domingo
  const primeiroDomingo = diaSemanaPrimeiro === 0 ? 1 : (8 - diaSemanaPrimeiro);
  return primeiroDomingo + 7;
}

function isDiaDaMulherHoje(){
  const hoje = new Date();
  return hoje.getMonth() === 2 && hoje.getDate() === 8; // 8 de março
}

function isDiaDasMaesHoje(){
  const hoje = new Date();
  return hoje.getMonth() === 4 && hoje.getDate() === segundoDomingoDeMaio(hoje.getFullYear());
}

// ===== MÉTRICAS DE NEGÓCIO (painel do Personal) =====
// Faturamento do mês é calculado a partir dos planos fechados dentro do mês corrente,
// já que ainda não existe um livro-caixa separado no sistema. Reflete fechamentos reais, não um valor inventado.
function calcularMetricasNegocio(){
  const naoLead = alunasPersonal.filter(function(a){ return a.status !== 'lead'; });
  const clientesAtivos = naoLead.filter(function(a){ return statusDoPlano(a) === 'ativas' || statusDoPlano(a) === 'porvencer'; }).length;

  const hoje = new Date();
  const mesAtual = hoje.getMonth(), anoAtual = hoje.getFullYear();
  const faturamentoMes = naoLead.reduce(function(soma, a){
    if(!a.dataFechouPlano || a.valorPlano == null) return soma;
    const dataFechou = new Date(a.dataFechouPlano + 'T00:00:00');
    if(dataFechou.getMonth() === mesAtual && dataFechou.getFullYear() === anoAtual) return soma + a.valorPlano;
    return soma;
  }, 0);

  const taxaRetencao = naoLead.length > 0 ? Math.round((clientesAtivos / naoLead.length) * 100) : 0;

  return { clientesAtivos: clientesAtivos, faturamentoMes: faturamentoMes, taxaRetencao: taxaRetencao };
}

function renderMetricasNegocio(){
  const area = document.getElementById('metricas-negocio-area');
  if(!area) return;
  const m = calcularMetricasNegocio();
  const faturamentoFormatado = m.faturamentoMes.toLocaleString('pt-BR', { minimumFractionDigits: 2, maximumFractionDigits: 2 });
  area.innerHTML =
    '<div class="alert-row" style="grid-template-columns:1fr 1fr 1fr;margin-bottom:14px;">' +
      '<div class="alert-stat"><p class="num">' + m.clientesAtivos + '</p><p class="lbl2">Clientes ativos</p></div>' +
      '<div class="alert-stat"><p class="num">R$ ' + faturamentoFormatado + '</p><p class="lbl2">Faturamento do mês</p></div>' +
      '<div class="alert-stat"><p class="num">' + m.taxaRetencao + '%</p><p class="lbl2">Taxa de retenção</p></div>' +
    '</div>' +
    renderMetaFinanceiraConteudo(m);
}

function renderMetaFinanceiraConteudo(m){
  if(!metaFaturamentoMensal || metaFaturamentoMensal <= 0){
    return '<div class="info-box" style="margin-bottom:14px;">' +
      '<p class="lbl" style="margin-bottom:8px;">Meta do mês</p>' +
      '<div class="form-group"><input class="form-input" id="input-meta-faturamento" type="number" step="0.01" placeholder="Quanto você quer faturar esse mês?"></div>' +
      '<button class="btn-gold" style="background:var(--card-2);color:var(--gold-soft);border:1px solid var(--border);" onclick="salvarMetaFaturamento()">Definir meta</button>' +
    '</div>';
  }

  const hoje = new Date();
  const ultimoDiaMes = new Date(hoje.getFullYear(), hoje.getMonth() + 1, 0).getDate();
  const diasRestantes = Math.max(0, ultimoDiaMes - hoje.getDate());
  const pct = Math.min(100, Math.round((m.faturamentoMes / metaFaturamentoMensal) * 100));
  const faltam = Math.max(0, metaFaturamentoMensal - m.faturamentoMes);
  const porDia = diasRestantes > 0 ? (faltam / diasRestantes) : faltam;

  const sugestoes = [];
  if(pct < 30 && diasRestantes > 15){
    sugestoes.push({ titulo: 'Hora de acelerar!', texto: 'Você está no início do mês. Foque em fechar novos clientes pra ganhar momentum.', impacto: 'Alto', esforco: 'Médio' });
  }
  if(diasRestantes <= 10 && pct < 100){
    sugestoes.push({ titulo: 'Sprint final!', texto: 'Restam ' + diasRestantes + ' dias. Considere promoções de última hora.', impacto: 'Alto', esforco: 'Baixo' });
  }
  const dadosAvisos = calcularCentralDeAvisos();
  if(dadosAvisos.planoVencido.length > 0){
    sugestoes.push({ titulo: 'Prospecção ativa', texto: 'Entre em contato com as ' + dadosAvisos.planoVencido.length + ' aluna(s) com plano vencido que ainda não renovaram.', impacto: 'Alto', esforco: 'Médio' });
  }
  if(m.clientesAtivos > 0 && pct < 100){
    sugestoes.push({ titulo: 'Upsell pra clientes atuais', texto: 'Ofereça serviços adicionais ou planos premium pra base ativa que você já tem.', impacto: 'Alto', esforco: 'Baixo' });
  }

  return '<div class="info-box" style="margin-bottom:14px;">' +
    '<p class="lbl" style="margin-bottom:2px;">Meta do mês</p>' +
    '<p class="txt" style="font-size:11px;color:var(--text-faint);margin-bottom:10px;">R$ ' + m.faturamentoMes.toLocaleString('pt-BR',{minimumFractionDigits:2}) + ' de R$ ' + metaFaturamentoMensal.toLocaleString('pt-BR',{minimumFractionDigits:2}) + '</p>' +
    '<div style="background:var(--card-2);border-radius:8px;height:10px;overflow:hidden;margin-bottom:10px;"><div style="background:linear-gradient(90deg,#F4D9A5,#E8C58A);height:100%;width:' + pct + '%;"></div></div>' +
    '<div class="row" style="gap:8px;margin-bottom:8px;">' +
      '<span class="tag">' + pct + '% da meta</span>' +
      '<span class="tag">' + diasRestantes + ' dias restantes</span>' +
      '<span class="tag">R$ ' + porDia.toLocaleString('pt-BR',{maximumFractionDigits:0}) + '/dia pra bater</span>' +
    '</div>' +
    '<span class="chip" style="cursor:pointer;" onclick="editarMetaFaturamento()">Editar meta</span>' +
  '</div>' +
  (sugestoes.length > 0 ? '<p class="section-label">Sugestões pra bater a meta</p>' +
    sugestoes.map(function(s){
      return '<div class="info-box" style="margin-bottom:8px;">' +
        '<p class="lbl" style="margin-bottom:2px;">' + s.titulo + '</p>' +
        '<p class="txt" style="font-size:12px;margin-bottom:6px;">' + s.texto + '</p>' +
        '<span class="tag" style="margin-right:6px;">Impacto ' + s.impacto + '</span><span class="tag">Esforço ' + s.esforco + '</span>' +
      '</div>';
    }).join('') : '');
}

async function salvarMetaFaturamento(){
  const input = document.getElementById('input-meta-faturamento');
  const valor = parseFloat(input.value);
  if(!valor || valor <= 0){ alert('Digita um valor de meta válido.'); return; }
  metaFaturamentoMensal = valor;
  await salvarCatalogoPersonal('config_meta_financeira', 'meta', { meta: metaFaturamentoMensal });
  renderMetricasNegocio();
}

function editarMetaFaturamento(){
  metaFaturamentoMensal = 0; // volta pro formulário de definir meta
  renderMetricasNegocio();
}

function calcularCentralDeAvisos(){
  const risco = alunasPersonal.filter(function(a){ return a.status !== 'lead' && a.telefone && detectarRiscoAbandonoPorConstancia(a.nome); });
  const avaliacaoAtrasada = alunasPersonal.filter(function(a){ const c = calcularContagemRegressivaAvaliacao(a); return a.status !== 'lead' && a.telefone && c && c.atrasada; });
  const planoVencendo = alunasPersonal.filter(function(a){ return a.telefone && statusDoPlano(a) === 'porvencer'; });
  const planoVencido = alunasPersonal.filter(function(a){ return a.telefone && statusDoPlano(a) === 'vencidas'; });
  const aniversario = alunasAniversarioHoje();
  return { risco: risco, avaliacaoAtrasada: avaliacaoAtrasada, planoVencendo: planoVencendo, planoVencido: planoVencido, aniversario: aniversario };
}

function renderCentralDeAvisos(){
  const area = document.getElementById('central-avisos-area');
  if(!area) return;
  const dados = calcularCentralDeAvisos();

  function linhaAviso(titulo, lista, tipo){
    return '<div class="list-item" style="margin-bottom:6px;">' +
      '<span>' + titulo + ' <span class="tag">' + lista.length + '</span></span>' +
      (lista.length > 0 ? '<span class="acao-pill" onclick="enviarAvisosEmLote(\'' + tipo + '\')">Enviar avisos</span>' : '') +
    '</div>';
  }

  const rankingAtual = calcularRankingComunidade();
  const metaBatida = rankingAtual.totalComunidade >= metaComunidadePontos;
  const eDiaDaMulher = isDiaDaMulherHoje();
  const eDiaDasMaes = isDiaDasMaesHoje();
  const totalComTelefone = alunasPersonal.filter(function(a){ return a.status !== 'lead' && a.telefone; }).length;

  area.innerHTML =
    linhaAviso('Risco de abandono', dados.risco, 'riscoAbandono') +
    linhaAviso('Avaliação física atrasada', dados.avaliacaoAtrasada, 'avaliacaoAtrasada') +
    linhaAviso('Plano vencendo em breve', dados.planoVencendo, 'planoVencendo') +
    linhaAviso('Plano já vencido', dados.planoVencido, 'planoVencido') +
    linhaAviso('Aniversário hoje', dados.aniversario, 'aniversario') +
    (metaBatida ? '<div class="list-item" style="margin-bottom:6px;background:var(--success-soft);"><span>🎉 Meta da comunidade batida!</span><span class="acao-pill" onclick="avisarMetaComunidadeBatida()">Avisar todas</span></div>' : '') +
    (eDiaDaMulher ? '<div class="list-item" style="margin-bottom:6px;background:var(--success-soft);"><span>💐 Hoje é Dia da Mulher <span class="tag">' + totalComTelefone + '</span></span><span class="acao-pill" onclick="avisarDataEspecial(\'diaDaMulher\')">Avisar todas</span></div>' : '') +
    (eDiaDasMaes ? '<div class="list-item" style="margin-bottom:6px;background:var(--success-soft);"><span>💐 Hoje é Dia das Mães <span class="tag">' + totalComTelefone + '</span></span><span class="acao-pill" onclick="avisarDataEspecial(\'diaDasMaes\')">Avisar todas</span></div>' : '') +
    '<div id="resultado-avisos-lote"></div>';
}

async function avisarDataEspecial(tipo){
  const elegiveis = alunasPersonal.filter(function(a){ return a.status !== 'lead' && a.telefone; });
  if(!confirm('Vai avisar ' + elegiveis.length + ' aluna(s) sobre essa data. Continuar?')) return;
  const area = document.getElementById('resultado-avisos-lote');
  let enviadas = 0, erros = [];
  for(let i = 0; i < elegiveis.length; i++){
    area.innerHTML = '<p class="txt" style="color:var(--text-faint);">Enviando ' + (i+1) + ' de ' + elegiveis.length + '...</p>';
    const saida = await enviarWhatsApp(elegiveis[i].telefone, templatesAvisos[tipo](elegiveis[i]));
    if(saida.sucesso) enviadas++; else erros.push({ nome: elegiveis[i].nome, motivo: saida.motivo });
    await new Promise(function(r){ setTimeout(r, 150); });
  }
  const listaErros = erros.length ? '<ul style="margin:8px 0 0;padding-left:18px;font-size:11px;color:var(--text-faint);">' + erros.map(function(e){ return '<li>' + e.nome + ': ' + (e.motivo || 'motivo não informado') + '</li>'; }).join('') + '</ul>' : '';
  area.innerHTML = '<div class="info-box" style="border-color:' + (erros.length ? '#C9784A' : 'var(--success)') + ';"><p class="txt">' + (erros.length ? '' : '✓ ') + enviadas + ' avisada(s)' + (erros.length ? ' · ' + erros.length + ' com erro' : '.') + '</p>' + listaErros + '</div>';
}

async function avisarMetaComunidadeBatida(){
  const elegiveis = alunasPersonal.filter(function(a){ return a.status !== 'lead' && a.telefone; });
  if(!confirm('Vai avisar ' + elegiveis.length + ' aluna(s) que a meta foi batida. Continuar?')) return;
  const area = document.getElementById('resultado-avisos-lote');
  let enviadas = 0;
  for(let i = 0; i < elegiveis.length; i++){
    area.innerHTML = '<p class="txt" style="color:var(--text-faint);">Enviando ' + (i+1) + ' de ' + elegiveis.length + '...</p>';
    const saida = await enviarWhatsApp(elegiveis[i].telefone, templatesAvisos.metaComunidade(metaComunidadePontos));
    if(saida.sucesso) enviadas++;
    await new Promise(function(r){ setTimeout(r, 150); });
  }
  area.innerHTML = '<div class="info-box" style="border-color:var(--success);"><p class="txt">✓ ' + enviadas + ' avisada(s) sobre a meta batida.</p></div>';
}

async function enviarAvisosEmLote(tipo){
  const dados = calcularCentralDeAvisos();
  const mapaLista = { riscoAbandono: dados.risco, avaliacaoAtrasada: dados.avaliacaoAtrasada, planoVencendo: dados.planoVencendo, planoVencido: dados.planoVencido, aniversario: dados.aniversario };
  const lista = mapaLista[tipo];
  if(!lista || lista.length === 0) return;
  if(!confirm('Vai mandar mensagem no WhatsApp de ' + lista.length + ' aluna(s) agora. Continuar?')) return;

  const resultadoArea = document.getElementById('resultado-avisos-lote');
  let enviadas = 0, erros = [];
  for(let i = 0; i < lista.length; i++){
    const a = lista[i];
    resultadoArea.innerHTML = '<p class="txt" style="color:var(--text-faint);">Enviando ' + (i+1) + ' de ' + lista.length + '...</p>';
    const mensagem = templatesAvisos[tipo](a);
    const saida = await enviarWhatsApp(a.telefone, mensagem);
    if(saida.sucesso) enviadas++; else erros.push({ nome: a.nome, motivo: saida.motivo });
    await new Promise(function(r){ setTimeout(r, 150); }); // pequena pausa entre envios, evita parecer spam
  }
  const listaErros = erros.length ? '<ul style="margin:8px 0 0;padding-left:18px;font-size:11px;color:var(--text-faint);">' + erros.map(function(e){ return '<li>' + e.nome + ': ' + (e.motivo || 'motivo não informado') + '</li>'; }).join('') + '</ul>' : '';
  resultadoArea.innerHTML = '<div class="info-box" style="border-color:' + (erros.length ? '#C9784A' : 'var(--success)') + ';margin-top:8px;"><p class="txt">' + (erros.length ? '' : '✓ ') + enviadas + ' enviada(s)' + (erros.length ? ' · ' + erros.length + ' com erro' : '') + '</p>' + listaErros + '</div>';
}

function mostrarFormularioMensagemEmMassa(){
  const area = document.getElementById('mensagem-massa-area');
  if(area.innerHTML){ area.innerHTML = ''; return; }
  area.innerHTML = '<div class="info-box">' +
    '<div class="form-group"><label class="form-label">Quem recebe?</label>' +
      '<select class="form-select" id="massa-publico">' +
        '<option value="todas">Todas as alunas</option>' +
        '<option value="ativas">Só as ativas</option>' +
        '<option value="porvencer">Só com plano vencendo</option>' +
        '<option value="vencidas">Só com plano vencido</option>' +
        '<option value="risco">Só em risco de abandono</option>' +
      '</select>' +
    '</div>' +
    '<div class="form-group"><label class="form-label">Mensagem</label><textarea class="form-input" id="massa-texto" rows="4" placeholder="Escreve aqui... pode usar {nome} que substitui pelo primeiro nome de cada aluna"></textarea></div>' +
    '<button class="btn-gold" onclick="confirmarMensagemEmMassa()">Enviar pra todas</button>' +
  '</div>';
}

async function confirmarMensagemEmMassa(){
  const publico = document.getElementById('massa-publico').value;
  const texto = document.getElementById('massa-texto').value.trim();
  if(!texto){ alert('Escreve a mensagem primeiro.'); return; }

  let destinatarias = alunasPersonal.filter(function(a){ return a.status !== 'lead' && a.telefone; });
  if(publico === 'ativas') destinatarias = destinatarias.filter(function(a){ return statusDoPlano(a) === 'ativas'; });
  else if(publico === 'porvencer') destinatarias = destinatarias.filter(function(a){ return statusDoPlano(a) === 'porvencer'; });
  else if(publico === 'vencidas') destinatarias = destinatarias.filter(function(a){ return statusDoPlano(a) === 'vencidas'; });
  else if(publico === 'risco') destinatarias = destinatarias.filter(function(a){ return detectarRiscoAbandonoPorConstancia(a.nome); });

  if(!confirm('Vai mandar essa mensagem pra ' + destinatarias.length + ' aluna(s), pelo WhatsApp. Continuar?')) return;

  const area = document.getElementById('mensagem-massa-area');
  let enviadas = 0, erros = [];
  for(let i = 0; i < destinatarias.length; i++){
    const a = destinatarias[i];
    area.innerHTML = '<p class="txt" style="color:var(--text-faint);">Enviando ' + (i+1) + ' de ' + destinatarias.length + '...</p>';
    const mensagemPersonalizada = texto.replace(/\{nome\}/g, a.nome.split(' ')[0]);
    const saida = await enviarWhatsApp(a.telefone, mensagemPersonalizada);
    if(saida.sucesso) enviadas++; else erros.push({ nome: a.nome, motivo: saida.motivo });
    await new Promise(function(r){ setTimeout(r, 150); });
  }
  area.innerHTML = '<div class="info-box" style="border-color:' + (erros.length ? '#C9784A' : 'var(--success)') + ';"><p class="lbl" style="color:' + (erros.length ? '#C9784A' : 'var(--success)') + ';">' + (erros.length ? 'Concluído com erros' : '✓ Concluído') + '</p><p class="txt">' + enviadas + ' enviada(s)' + (erros.length ? ' · ' + erros.length + ' com erro' : '') + '</p>' + (erros.length ? '<ul style="margin:8px 0 0;padding-left:18px;font-size:11px;color:var(--text-faint);">' + erros.map(function(e){ return '<li>' + e.nome + ': ' + (e.motivo || 'motivo não informado') + '</li>'; }).join('') + '</ul>' : '') + '</div>';
}

// ===== ENVIO DE WHATSAPP (via porteiro seguro no Supabase) =====
async function enviarWhatsApp(telefone, mensagem, jaTentouDeNovo){
  if(!telefone) return { sucesso: false, motivo: 'sem telefone cadastrado' };
  try {
    const resposta = await fetch(SUPABASE_URL + '/functions/v1/enviar-whatsapp', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json', 'apikey': SUPABASE_ANON_KEY, 'Authorization': 'Bearer ' + SUPABASE_ANON_KEY },
      body: JSON.stringify({ telefone: telefone, mensagem: mensagem })
    });
    const dados = await resposta.json();
    if(!resposta.ok || dados.error) return { sucesso: false, motivo: dados.error || ('HTTP ' + resposta.status) };
    return { sucesso: true };
  } catch(erro){
    // Instabilidade passageira de rede: tenta mais uma vez antes de desistir de vez
    if(!jaTentouDeNovo){
      await new Promise(function(r){ setTimeout(r, 1500); });
      return enviarWhatsApp(telefone, mensagem, true);
    }
    return { sucesso: false, motivo: erro.message };
  }
}

async function testarConexaoWhatsApp(){
  const area = document.getElementById('resultado-teste-whatsapp');
  if(!area) return;
  area.innerHTML = '<p class="txt" style="color:var(--text-faint);">Verificando conexão com o Z-API...</p>';
  try {
    const resposta = await fetch(SUPABASE_URL + '/functions/v1/enviar-whatsapp', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json', 'apikey': SUPABASE_ANON_KEY, 'Authorization': 'Bearer ' + SUPABASE_ANON_KEY },
      body: JSON.stringify({ verificarStatus: true })
    });
    const dados = await resposta.json();
    if(!resposta.ok || dados.error){
      area.innerHTML = '<div class="info-box" style="border-color:#C9784A;"><p class="txt" style="color:#C9784A;">Não consegui nem falar com o servidor: ' + (dados.error || ('HTTP ' + resposta.status)) + '</p></div>';
      return;
    }
    if(dados.conectado){
      area.innerHTML = '<div class="info-box" style="border-color:var(--success);"><p class="txt" style="color:var(--success);">✓ WhatsApp conectado e pronto pra enviar.</p></div>';
    } else {
      area.innerHTML = '<div class="info-box" style="border-color:#C9784A;"><p class="txt" style="color:#C9784A;">WhatsApp desconectado: ' + (dados.detalhe || 'precisa reconectar o número no painel da Z-API (escanear o QR code de novo).') + '</p></div>';
    }
  } catch(erro){
    area.innerHTML = '<div class="info-box" style="border-color:#C9784A;"><p class="txt" style="color:#C9784A;">Não consegui nem falar com o servidor: ' + erro.message + ' (provavelmente a Edge Function não está publicada, ou o link do Supabase no arquivo está errado).</p></div>';
  }
}

function detectarRiscoAbandonoPorConstancia(nomeAluna){
  const prog = getProgressoAluna(nomeAluna);
  const semanaAtual = prog.semana;
  if(semanaAtual < 3) return false; // não dá pra avaliar "3 semanas seguidas" antes da 3ª semana
  const totalPlanejadoPorSemana = totalDiasDeTreino();
  if(totalPlanejadoPorSemana === 0) return false;

  for(let s = semanaAtual - 2; s <= semanaAtual; s++){
    const concluidosNaSemana = (prog.diasConcluidos[s] || []).length;
    const pctNaSemana = concluidosNaSemana / totalPlanejadoPorSemana;
    if(pctNaSemana >= 0.4) return false; // uma semana boa no meio já quebra o padrão de "3 seguidas"
  }
  return true;
}

function definirEstrategiaAtual(a){
  const classificacao = classificarAluna(a);
  const faseInfo = determinarFaseMacrociclo(a);
  const prog = getProgressoAluna(a.nome);
  const objetivoLower = (a.objetivo || '').toLowerCase();

  const semanasComDado = Object.keys(prog.diasConcluidos || {}).map(Number).sort(function(x,y){ return y-x; });
  const semanaMaisRecente = semanasComDado[0];
  const teveGapRecente = semanaMaisRecente != null && (prog.semana - semanaMaisRecente) >= 2;
  if(teveGapRecente){
    return {
      nome: 'Retorno pós-pausa',
      motivo: 'Ficou ' + (prog.semana - semanaMaisRecente) + ' semana(s) sem registro de treino.',
      volumeMultiplicador: 0.7, intensidadeAlvo: 'baixa', prioridade: 'reconstruir consistência e técnica'
    };
  }

  if(classificacao.fadiga.nivel === 'alto' || (classificacao.constanciaPct != null && classificacao.constanciaPct < 50)){
    return {
      nome: 'Manutenção',
      motivo: classificacao.fadiga.nivel === 'alto' ? 'Sinais de fadiga acumulada: ' + classificacao.fadiga.motivos.join('; ') + '.' : 'Aderência abaixo de 50% recentemente.',
      volumeMultiplicador: 0.85, intensidadeAlvo: 'moderada', prioridade: 'sustentar sem sobrecarregar'
    };
  }

  const palavrasEmagrecimento = ['emagrec', 'definir', 'definição', 'perder peso', 'perdi peso', 'gordura', 'secar'];
  if(palavrasEmagrecimento.some(function(p){ return objetivoLower.indexOf(p) !== -1; })){
    return {
      nome: 'Redução de gordura',
      motivo: 'Objetivo relatado na anamnese inclui emagrecimento/definição.',
      volumeMultiplicador: 1.0, intensidadeAlvo: 'moderada-alta', prioridade: 'densidade de treino, preservar massa magra'
    };
  }

  if(classificacao.nivel === 'Avançado' && faseInfo.blocoTecnico !== 'deload' && classificacao.grupoPrioritario){
    return {
      nome: 'Especialização de ' + classificacao.grupoPrioritario,
      motivo: 'Nível avançado, sem sinais de fadiga, com ênfase clara na anamnese.',
      volumeMultiplicador: 1.15, intensidadeAlvo: 'alta', prioridade: 'volume extra no grupo prioritário'
    };
  }

  return {
    nome: 'Hipertrofia',
    motivo: 'Estratégia padrão pro momento atual dela, sem sinal de precisar de ajuste especial.',
    volumeMultiplicador: 1.0, intensidadeAlvo: 'moderada', prioridade: 'ganho de massa muscular geral'
  };
}

function calcularBlocoAtual(a){
  if(a.nivel === 'Iniciante') return { bloco: 'Volume único (pirâmide de base)', descricao: 'Foco em progressão contínua de carga e técnica, sem alternância de blocos ainda.', blocoTecnico: 'base' };
  const fadiga = avaliarSinaisDeFadiga(a);
  if(fadiga.nivel === 'alto'){
    return { bloco: 'Deload (antecipado por fadiga)', descricao: 'Sinais de fadiga acumulada detectados: ' + fadiga.motivos.join('; ') + '. Volume reduzido nessa semana pra recuperar.', blocoTecnico: 'deload' };
  }
  const prog = getProgressoAluna(a.nome);
  const ciclosCompletos = Math.floor((prog.semana - 1) / 5); // ciclo de reavaliação ~5 semanas
  const posicaoNoPadrao = ciclosCompletos % 4;
  if(posicaoNoPadrao === 2){
    return { bloco: 'Bloco de Choque', descricao: 'Intensidade alta, volume mais baixo (2-3 semanas), foco em força e quebra de rotina.', blocoTecnico: 'choque' };
  }
  if(posicaoNoPadrao === 3){
    return { bloco: 'Deload', descricao: 'Semana de recuperação ativa: volume reduzido, intensidade baixa, longe da falha. Sem métodos combinados.', blocoTecnico: 'deload' };
  }
  if(fadiga.nivel === 'moderado'){
    return { bloco: 'Bloco de Base (volume reduzido por fadiga)', descricao: 'Sinais moderados de fadiga: ' + fadiga.motivos.join('; ') + '. Volume ajustado pra baixo nessa semana.', blocoTecnico: 'base', volumeReduzido: true };
  }
  return { bloco: 'Bloco de Base', descricao: 'Volume alto, intensidade moderada (4-6 semanas), foco em capacidade de trabalho e hipertrofia.', blocoTecnico: 'base' };
}

// ===== MOTOR 3: PERIODIZAÇÃO — fases nomeadas do macrociclo =====
// Não substitui calcularBlocoAtual (que já decide o volume/intensidade técnica corretamente).
// Isso só adiciona a camada de nome de fase macro, como um treinador pensaria: Adaptação -> Base -> Especialização -> Deload.
function determinarFaseMacrociclo(a){
  const prog = getProgressoAluna(a.nome);
  const blocoInfo = calcularBlocoAtual(a);

  if(prog.semana <= 2){
    return { fase: 'Adaptação', blocoTecnico: blocoInfo.blocoTecnico, descricao: 'Primeiras semanas: foco em aprendizado técnico e adaptação neuromuscular, volume controlado.', volumeReduzido: true };
  }

  const nomesPorBlocoTecnico = {
    'base': 'Base de Hipertrofia',
    'choque': 'Especialização / Intensificação',
    'deload': 'Deload'
  };

  return {
    fase: nomesPorBlocoTecnico[blocoInfo.blocoTecnico] || 'Base de Hipertrofia',
    blocoTecnico: blocoInfo.blocoTecnico,
    descricao: blocoInfo.descricao,
    volumeReduzido: !!blocoInfo.volumeReduzido
  };
}

/* ===== PROMOÇÃO DE NÍVEL (iniciante → intermediário) ===== */

const SEMANAS_MINIMAS_INICIANTE = 12;

function calcularElegibilidadePromocaoNivel(nome){
  const a = alunasPersonal.find(function(x){ return x.nome === nome; });
  if(!a || a.nivel !== 'Iniciante') return null;
  const prog = getProgressoAluna(nome);
  const stats = calcularEstatisticasAluna(nome);
  const pctConstancia = stats.temDados ? Math.round((stats.totalConcluidos / stats.totalPlanejado) * 100) : 0;
  const tecnicasAprovadas = a.tecnicaAprovada ? Object.keys(a.tecnicaAprovada).filter(function(k){ return a.tecnicaAprovada[k] === 'aprovado'; }).length : 0;

  const criterios = [
    { nome: '12 semanas mínimas no programa', atingido: prog.semana >= SEMANAS_MINIMAS_INICIANTE, detalhe: prog.semana + ' de ' + SEMANAS_MINIMAS_INICIANTE + ' semanas' },
    { nome: 'Constância ≥ 70%', atingido: pctConstancia >= 70, detalhe: pctConstancia + '%' },
    { nome: 'Técnica aprovada em ao menos 1 exercício principal', atingido: tecnicasAprovadas >= 1, detalhe: tecnicasAprovadas + ' exercício(s) aprovado(s)' },
    { nome: 'Progressões de carga reais registradas', atingido: stats.progressoes >= 1, detalhe: stats.progressoes + ' progressão(ões)' }
  ];
  return { elegivel: criterios.every(function(c){ return c.atingido; }), criterios: criterios };
}

function ajusteRecuperacaoPorIdade(idade){
  if(!idade) return { ajuste: 0, nota: '' };
  if(idade >= 45){
    return { ajuste: -8, nota: 'Nessa faixa etária, a queda de estrogênio típica da menopausa costuma estreitar a janela de recuperação, treino de força continua sendo uma das melhores ferramentas pra essa fase, só vale dar mais atenção ao descanso entre sessões.' };
  }
  if(idade >= 40){
    return { ajuste: -4, nota: 'A partir dessa faixa etária é comum a perimenopausa começar a influenciar a recuperação, nada preocupante, só um sinal pra observar com mais atenção.' };
  }
  return { ajuste: 0, nota: '' };
}

// ===== TELA "MEU PROGRESSO" — cruza dados que já calculamos em outros lugares, numa visão única =====
// ===== RANKING DA COMUNIDADE (pontos coletivos) =====
let metaComunidadePontos = 50000; // meta padrão, Personal pode mudar
let metaComunidadeRecompensa = 'Em breve — a Personal vai definir o que desbloqueia quando a comunidade bater a meta';
let metaFaturamentoMensal = 0; // 0 = sem meta definida ainda

function calcularPontosAluna(nome){
  const prog = getProgressoAluna(nome);
  const historico = prog.dnaScoreHistorico || [];
  return historico.reduce(function(soma, r){ return soma + (r.score || 0); }, 0);
}

function calcularRankingComunidade(){
  const pontosTodas = alunasPersonal
    .filter(function(a){ return a.status !== 'lead'; })
    .map(function(a){ return { nome: a.nome, pontos: calcularPontosAluna(a.nome) }; })
    .filter(function(p){ return p.pontos > 0; });

  const totalComunidade = pontosTodas.reduce(function(soma, p){ return soma + p.pontos; }, 0);
  const pontosDela = calcularPontosAluna(NOME_ALUNA_LOGADA);
  const posicaoDela = pontosTodas.filter(function(p){ return p.pontos > pontosDela; }).length + 1;
  const totalComPontos = pontosTodas.length;
  const percentil = totalComPontos > 0 ? Math.round((1 - (posicaoDela - 1) / totalComPontos) * 100) : null;

  return { pontosDela: pontosDela, posicaoDela: posicaoDela, totalComPontos: totalComPontos, percentil: percentil, totalComunidade: totalComunidade };
}

function renderRanking(){
  const container = document.getElementById('area-ranking-conteudo');
  if(!container) return;
  const r = calcularRankingComunidade();
  const pctMeta = Math.min(100, Math.round((r.totalComunidade / metaComunidadePontos) * 100));

  let html = '<div class="dna-score-card"><div class="dna-score-left">' +
    '<p class="dna-score-label">SEUS PONTOS</p>' +
    '<p class="dna-score-numero">' + r.pontosDela + '</p>' +
    '<p class="dna-score-comparativo" style="color:var(--gold-soft);">' + (r.percentil != null ? '▲ Top ' + (100 - r.percentil + 1) + '% da consultoria' : 'Ainda calculando sua posição') + '</p>' +
  '</div></div>';

  html += '<p class="section-label" style="margin-top:20px;">Meta da comunidade</p>' +
    '<div class="info-box">' +
      '<p class="txt"><b>' + r.totalComunidade.toLocaleString('pt-BR') + '</b> de ' + metaComunidadePontos.toLocaleString('pt-BR') + ' pontos</p>' +
      '<div style="background:var(--card-2);border-radius:8px;height:10px;margin:8px 0;overflow:hidden;"><div style="background:linear-gradient(90deg,#F4D9A5,#E8C58A);height:100%;width:' + pctMeta + '%;"></div></div>' +
      '<p class="txt" style="font-size:11px;color:var(--text-faint);">' + pctMeta + '% da meta · quando bater 100%: ' + metaComunidadeRecompensa + '</p>' +
    '</div>' +
    '<p class="txt" style="font-size:11px;color:var(--text-faint);margin-top:10px;">Seus pontos somam junto com as outras alunas da consultoria — a gente não mostra nome nem posição de ninguém além da sua, mas cada treino registrado ajuda a comunidade toda a chegar mais perto.</p>';

  container.innerHTML = html;
}

function renderMeuProgresso(){
  const container = document.getElementById('area-progresso-conteudo');
  if(!container) return;
  const nome = NOME_ALUNA_LOGADA;
  const a = alunasPersonal.find(function(x){ return x.nome === nome; });
  if(!a){ container.innerHTML = ''; return; }

  try {
    renderMeuProgressoConteudo(container, nome, a);
  } catch(erroReal){
    console.error('[Meu Progresso] erro real, capturado pra não travar a tela:', erroReal);
    container.innerHTML = '<p class="txt" style="color:var(--text-faint);">Ainda reunindo os dados da sua evolução. Volta aqui em alguns dias, conforme for registrando treinos.</p>';
  }
}

// ===== RODA DA VIDA (autoavaliação mensal em 10 áreas) =====
const AREAS_RODA_DA_VIDA = [
  { id: 'saude', label: 'Saúde e corpo', icone: 'ti-heartbeat' },
  { id: 'trabalho', label: 'Trabalho e carreira', icone: 'ti-briefcase' },
  { id: 'financeiro', label: 'Financeiro', icone: 'ti-coin' },
  { id: 'amoroso', label: 'Relacionamento amoroso', icone: 'ti-heart' },
  { id: 'familia', label: 'Família', icone: 'ti-home' },
  { id: 'amigos', label: 'Amigos e vida social', icone: 'ti-users' },
  { id: 'mente', label: 'Mente e aprendizado', icone: 'ti-bulb' },
  { id: 'espiritualidade', label: 'Espiritualidade e propósito', icone: 'ti-flower' },
  { id: 'realizacao', label: 'Realização pessoal', icone: 'ti-trophy' },
  { id: 'descanso', label: 'Descanso e lazer', icone: 'ti-beach' }
];

function getMesAtualISO(){
  const d = new Date();
  return d.getFullYear() + '-' + String(d.getMonth() + 1).padStart(2, '0');
}

function getRodaDaVidaMes(nome, mesISO){
  const prog = getProgressoAluna(nome);
  if(!prog.rodaDaVida) prog.rodaDaVida = {};
  if(!prog.rodaDaVida[mesISO]) prog.rodaDaVida[mesISO] = { mes: mesISO, areas: {} };
  return prog.rodaDaVida[mesISO];
}

function getUltimaRodaDaVidaPreenchida(nome){
  const prog = getProgressoAluna(nome);
  if(!prog.rodaDaVida) return null;
  const meses = Object.keys(prog.rodaDaVida).sort();
  if(meses.length === 0) return null;
  return prog.rodaDaVida[meses[meses.length - 1]];
}

function salvarCampoRodaDaVida(nome, areaId, valor){
  const mesISO = getMesAtualISO();
  const registro = getRodaDaVidaMes(nome, mesISO);
  registro.areas[areaId] = valor;
  salvarProgressoNoSupabase(nome);
  renderRodaDaVidaAluna();
}

function calcularPontoForteEAreaFraca(areasObj){
  const preenchidas = AREAS_RODA_DA_VIDA.filter(function(a){ return areasObj[a.id] != null; });
  if(preenchidas.length === 0) return null;
  let forte = preenchidas[0], fraca = preenchidas[0];
  preenchidas.forEach(function(a){
    if(areasObj[a.id] > areasObj[forte.id]) forte = a;
    if(areasObj[a.id] < areasObj[fraca.id]) fraca = a;
  });
  return { forte: { label: forte.label, valor: areasObj[forte.id] }, fraca: { label: fraca.label, valor: areasObj[fraca.id] } };
}

function gerarSvgRadar(areasObj, tamanho){
  tamanho = tamanho || 260;
  const cx = tamanho / 2, cy = tamanho / 2, raioMax = tamanho / 2 - 34;
  const n = AREAS_RODA_DA_VIDA.length;
  function pontoNoAngulo(indice, raio){
    const angulo = (indice * (2 * Math.PI / n)) - (Math.PI / 2);
    return { x: cx + raio * Math.cos(angulo), y: cy + raio * Math.sin(angulo) };
  }

  let grades = '';
  [0.25, 0.5, 0.75, 1].forEach(function(fracao){
    const pontosGrade = AREAS_RODA_DA_VIDA.map(function(a, i){ const p = pontoNoAngulo(i, raioMax * fracao); return p.x + ',' + p.y; }).join(' ');
    grades += '<polygon points="' + pontosGrade + '" fill="none" stroke="var(--border)" stroke-width="1"></polygon>';
  });

  const pontosValor = AREAS_RODA_DA_VIDA.map(function(a, i){
    const valor = areasObj[a.id] != null ? areasObj[a.id] : 0;
    const p = pontoNoAngulo(i, raioMax * (valor / 5));
    return p.x + ',' + p.y;
  }).join(' ');

  const labels = AREAS_RODA_DA_VIDA.map(function(a, i){
    const p = pontoNoAngulo(i, raioMax + 16);
    return '<circle cx="' + p.x + '" cy="' + p.y + '" r="3" fill="var(--gold-soft)"></circle>';
  }).join('');

  return '<svg viewBox="0 0 ' + tamanho + ' ' + tamanho + '" width="100%" style="max-width:280px;display:block;margin:0 auto;">' +
    grades +
    '<polygon points="' + pontosValor + '" fill="rgba(232,197,138,0.28)" stroke="var(--gold-soft)" stroke-width="2"></polygon>' +
    labels +
    '</svg>';
}

function renderRodaDaVidaAluna(){
  const container = document.getElementById('area-rodadavida-conteudo');
  if(!container) return;
  const nome = NOME_ALUNA_LOGADA;
  const mesISO = getMesAtualISO();
  const registro = getRodaDaVidaMes(nome, mesISO);
  const nomeDoMes = new Date(mesISO + '-01T00:00:00').toLocaleDateString('pt-BR', { month: 'long', year: 'numeric' });
  const totalPreenchido = AREAS_RODA_DA_VIDA.filter(function(a){ return registro.areas[a.id] != null; }).length;

  let html = '<p class="section-label" style="margin-top:0;">' + nomeDoMes.charAt(0).toUpperCase() + nomeDoMes.slice(1) + ' <span class="tag">' + totalPreenchido + ' de ' + AREAS_RODA_DA_VIDA.length + '</span></p>';

  const jaPreencheuAlgumaVez = !!getUltimaRodaDaVidaPreenchida(nome);
  if(!jaPreencheuAlgumaVez){
    html += '<div class="info-box" style="margin-bottom:16px;background:var(--success-soft);border-color:var(--success);"><p class="txt" style="font-size:12px;">A Roda da Vida avalia como está sua vida além do treino, saúde, trabalho, família, financeiro e mais. Isso ajuda seu personal a te entender melhor como um todo, não só o que acontece na academia. Leva menos de 2 minutos, uma vez por mês.</p></div>';
  }

  if(totalPreenchido > 0){
    html += '<div class="info-box" style="margin-bottom:16px;">' + gerarSvgRadar(registro.areas) + '</div>';
    const resumo = calcularPontoForteEAreaFraca(registro.areas);
    if(resumo){
      html += '<div class="row" style="gap:8px;margin-bottom:16px;">' +
        '<div class="info-box" style="flex:1;background:var(--success-soft);border-color:var(--success);"><p class="lbl" style="font-size:11px;">Ponto forte</p><p class="txt" style="font-size:12px;">' + resumo.forte.label + ' (' + resumo.forte.valor + '/5)</p></div>' +
        '<div class="info-box" style="flex:1;"><p class="lbl" style="font-size:11px;">Área pra focar</p><p class="txt" style="font-size:12px;">' + resumo.fraca.label + ' (' + resumo.fraca.valor + '/5)</p></div>' +
      '</div>';
    }
  }

  AREAS_RODA_DA_VIDA.forEach(function(a){
    const valorAtual = registro.areas[a.id];
    html += '<div class="info-box" style="margin-bottom:8px;">' +
      '<p class="txt" style="font-size:12px;margin-bottom:6px;"><i class="ti ' + a.icone + '" style="margin-right:6px;color:var(--gold-soft);"></i>' + a.label + '</p>' +
      '<div class="row" style="gap:6px;">' +
        [0,1,2,3,4,5].map(function(n){
          return '<span class="chip" style="' + (valorAtual === n ? 'background:var(--gold-soft);color:#1A1409;border-color:var(--gold-soft);' : '') + '" onclick="salvarCampoRodaDaVida(\'' + nome.replace(/'/g,"\\'") + '\',\'' + a.id + '\',' + n + ')">' + n + '</span>';
        }).join('') +
      '</div>' +
    '</div>';
  });

  container.innerHTML = html;
}

function renderMeuProgressoConteudo(container, nome, a){
  const score = calcularProbabilidadeSucesso(nome);
  const comparativoScore = calcularComparativoScore(nome);
  const prog = getProgressoAluna(nome);
  const comparativoSemana = calcularComparativoSemanal(nome);

  let html = '';

  // DNA Score + comparativo real (o que ela quer ver primeiro, ao abrir a tela)
  html += '<div class="dna-score-card" onclick="openDetail(\'dna\')">' +
    '<div class="dna-score-left">' +
      '<p class="dna-score-label">DNA SCORE</p>' +
      '<p class="dna-score-numero">' + (score != null ? score : '--') + '</p>' +
      '<p class="dna-score-comparativo" style="color:' + (comparativoScore != null && comparativoScore > 0 ? 'var(--success)' : 'var(--text-faint)') + ';">' +
        (comparativoScore == null ? 'Aguardando dados suficientes' : (comparativoScore > 0 ? '▲ +' : (comparativoScore < 0 ? '▼ ' : '')) + comparativoScore + '% vs. semana anterior') +
      '</p>' +
    '</div>' +
  '</div>';

  // Check-in emocional do dia + hábitos com pontuação (o que ela preenche, vem depois do que ela olha)
  html += '<div id="checkin-habitos-area"></div>';

  // Linha do tempo de composição (peso/gordura), cruzando com o que já construímos
  html += '<p class="section-label" style="margin-top:20px;">Linha do tempo</p>';
  const historico = (a.composicaoHistorico || []).slice();
  if(a.composicaoAtual) historico.push(Object.assign({ semana: prog.semana, atual: true }, a.composicaoAtual));
  if(historico.length === 0){
    html += '<p class="txt" style="color:var(--text-faint);">Ainda sem registros de composição pra montar a linha do tempo. Registra na aba Composição corporal.</p>';
  } else {
    html += historico.map(function(reg, i){
      const label = reg.atual ? 'Hoje' : (reg.data || ('Semana ' + reg.semana));
      const partes = [];
      if(reg.peso != null) partes.push(String(reg.peso).replace('.',',') + ' kg');
      if(reg.gordura != null) partes.push(String(reg.gordura).replace('.',',') + '% gordura');
      return '<div style="display:flex;align-items:center;gap:10px;margin-bottom:10px;">' +
        '<div style="width:10px;height:10px;border-radius:50%;background:' + (reg.atual ? 'var(--gold-soft)' : 'var(--border-strong)') + ';flex-shrink:0;"></div>' +
        '<div><p style="font-size:12px;font-weight:600;margin:0;">' + label + '</p><p style="font-size:11px;color:var(--text-faint);margin:0;">' + (partes.join(' · ') || 'sem detalhe') + '</p></div>' +
      '</div>';
    }).join('');
  }

  // Evolução por pilares, reaproveitando o comparativo semanal já calculado
  html += '<p class="section-label" style="margin-top:20px;">Evolução por pilares</p>';
  const pilares = [
    { nome: 'Constância', valor: comparativoSemana.constancia },
    { nome: 'Progressão de carga', valor: comparativoSemana.progressao },
    { nome: 'Nutrição', valor: comparativoSemana.nutricao },
    { nome: 'Recuperação', valor: comparativoSemana.recuperacao }
  ];
  html += pilares.map(function(p){
    const semDado = p.valor == null || isNaN(p.valor);
    return '<div style="display:flex;justify-content:space-between;align-items:center;padding:10px 0;border-bottom:1px solid var(--border);">' +
      '<span style="font-size:13px;">' + p.nome + '</span>' +
      '<span style="font-size:13px;font-weight:600;color:' + (semDado ? 'var(--text-faint)' : (p.valor >= 0 ? 'var(--success)' : 'var(--gold-soft)')) + ';">' +
        (semDado ? 'sem dado ainda' : (p.valor > 0 ? '▲ +' : (p.valor < 0 ? '▼ ' : '')) + p.valor + '%') +
      '</span>' +
    '</div>';
  }).join('');

  // Gráfico simples de evolução do DNA Score
  html += '<p class="section-label" style="margin-top:20px;">Evolução do DNA Score</p>';
  const historicoScore = prog.dnaScoreHistorico || [];
  if(historicoScore.length < 2){
    html += '<p class="txt" style="color:var(--text-faint);">Ainda precisa de mais semanas registradas pra montar o gráfico de evolução.</p>';
  } else {
    const max = Math.max.apply(null, historicoScore.map(function(r){ return r.score; }));
    const min = Math.min.apply(null, historicoScore.map(function(r){ return r.score; }));
    const faixa = Math.max(1, max - min);
    html += '<div style="display:flex;align-items:flex-end;gap:6px;height:80px;padding:10px 0;">' +
      historicoScore.map(function(r){
        const alturaPct = 20 + ((r.score - min) / faixa) * 80;
        return '<div style="flex:1;display:flex;flex-direction:column;align-items:center;gap:4px;">' +
          '<div style="width:100%;height:' + alturaPct + 'px;background:linear-gradient(180deg,#F4D9A5,#E8C58A);border-radius:4px 4px 0 0;"></div>' +
          '<span style="font-size:9px;color:var(--text-faint);">S' + r.semana + '</span>' +
        '</div>';
      }).join('') +
    '</div>';
  }

  // Botão pro Shape Analysis, desligado por enquanto
  html += '<p class="section-label" style="margin-top:20px;">Shape Analysis</p>' +
    '<div class="info-box" style="opacity:0.6;">' +
      '<p class="lbl">Em breve</p>' +
      '<p class="txt">Comparação visual de fotos ao longo do tempo. Estamos preparando esse material — assim que estiver pronto, avisamos por aqui.</p>' +
    '</div>';

  container.innerHTML = html;
  renderCheckInEHabitos(nome);
}

function calcularEstatisticasAluna(nome){
  const prog = getProgressoAluna(nome);
  let totalConcluidos = 0, totalSemanas = 0;
  Object.keys(prog.diasConcluidos).forEach(function(s){
    totalConcluidos += prog.diasConcluidos[s].length;
    totalSemanas++;
  });
  const totalPlanejado = totalSemanas * totalDiasDeTreino();
  let progressoes = 0;
  Object.keys(prog.historico).forEach(function(ex){
    prog.historico[ex].forEach(function(r){ if(r.sugestao.texto === 'Aumentar carga') progressoes++; });
  });
  return { totalConcluidos: totalConcluidos, totalPlanejado: totalPlanejado, progressoes: progressoes, temDados: totalConcluidos > 0 };
}

function openDetail(type, arg){
  const el = document.getElementById('detail-content');

if(type === 'central'){
    const stats = calcularEstatisticasAluna(NOME_ALUNA_LOGADA);
    if(!stats.temDados){
      el.innerHTML =
        '<p class="page-sub" style="margin-top:14px;">Central de Inteligência</p>' +
        '<h1 class="page-title" style="margin-top:0;">Bom dia, ' + NOME_ALUNA_LOGADA.split(' ')[0] + '</h1>' +
        '<div class="ring-wrap">' +
          '<svg width="84" height="84" viewBox="0 0 96 96">' +
            '<circle cx="48" cy="48" r="40" fill="none" stroke="#26231C" stroke-width="8"/>' +
            '<circle cx="48" cy="48" r="40" fill="none" stroke="url(#goldring)" stroke-width="8" stroke-linecap="round" stroke-dasharray="251" stroke-dashoffset="251" transform="rotate(-90 48 48)"/>' +
            '<defs><linearGradient id="goldring" x1="0" y1="0" x2="1" y2="1"><stop offset="0%" stop-color="#F4D9A5"/><stop offset="100%" stop-color="#4C3E25"/></linearGradient></defs>' +
          '</svg>' +
          '<div><p class="ring-num" style="font-size:16px;">Calibrando</p><p class="ring-label">seu DNA MUSA está começando a te conhecer</p></div>' +
        '</div>' +
        '<div class="insight"><p>Hoje é seu primeiro dia, cada treino registrado deixa suas previsões mais precisas.</p></div>';
    } else {
      const probabilidade = calcularProbabilidadeSucesso(NOME_ALUNA_LOGADA);
      const offset = Math.round(251 - (251 * probabilidade / 100));
      const nutriStatsCentral = calcularNutricaoStats(NOME_ALUNA_LOGADA);
      let insightExtra = '';
      if(nutriStatsCentral.temDados && nutriStatsCentral.potencialGanhoGordura > 55){
        insightExtra = ' As fugas na dieta e/ou álcool das últimas semanas estão pesando nesse número, reduzir isso tende a acelerar os resultados que você quer ver.';
      } else if(nutriStatsCentral.temDados){
        insightExtra = ' Sua adesão à dieta está ajudando bastante nesse número.';
      }
      el.innerHTML =
        '<p class="page-sub" style="margin-top:14px;">Central de Inteligência</p>' +
        '<h1 class="page-title" style="margin-top:0;">Bom dia, ' + NOME_ALUNA_LOGADA.split(' ')[0] + '</h1>' +
        '<div class="ring-wrap">' +
          '<svg width="84" height="84" viewBox="0 0 96 96">' +
            '<circle cx="48" cy="48" r="40" fill="none" stroke="#26231C" stroke-width="8"/>' +
            '<circle cx="48" cy="48" r="40" fill="none" stroke="url(#goldring)" stroke-width="8" stroke-linecap="round" stroke-dasharray="251" stroke-dashoffset="' + offset + '" transform="rotate(-90 48 48)"/>' +
            '<defs><linearGradient id="goldring" x1="0" y1="0" x2="1" y2="1"><stop offset="0%" stop-color="#F4D9A5"/><stop offset="100%" stop-color="#4C3E25"/></linearGradient></defs>' +
          '</svg>' +
          '<div><p class="ring-num">' + probabilidade + '%</p><p class="ring-label">de probabilidade de sucesso</p></div>' +
        '</div>' +
        '<div class="insight"><p>Você completou ' + stats.totalConcluidos + ' de ' + stats.totalPlanejado + ' treinos planejados, e já teve ' + stats.progressoes + ' aumentos de carga registrados.' + insightExtra + '</p></div>';
    }
  } else if(type === 'dna'){
    const stats = calcularEstatisticasAluna(NOME_ALUNA_LOGADA);
    let indsHtml = '';
    if(!stats.temDados){
      ['Potencial de hipertrofia', 'Capacidade de recuperação', 'Constância', 'Resposta nutricional'].forEach(function(nome){
        indsHtml += '<div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:10px;"><span style="font-size:12px;color:var(--text-dim);">' + nome + '</span><span style="font-size:11px;color:var(--text-faint);border:1px dashed var(--border-strong);padding:3px 8px;border-radius:8px;">Calibrando</span></div>';
      });
    } else {
      const pctConstancia = Math.round((stats.totalConcluidos / stats.totalPlanejado) * 100);
      const alunaObj = alunasPersonal.find(function(a){ return a.nome === NOME_ALUNA_LOGADA; });
      const ajusteIdade = ajusteRecuperacaoPorIdade(alunaObj ? alunaObj.idade : null);
      const nutriStatsPreview = calcularNutricaoStats(NOME_ALUNA_LOGADA);
      const ajusteSonoPreview = nutriStatsPreview.temDados ? nutriStatsPreview.ajusteRecuperacaoSono : 0;
      const capRecuperacao = Math.max(20, Math.min(95, (stats.progressoes >= 2 ? 68 : 60) + ajusteIdade.ajuste + ajusteSonoPreview));
      const capProgressao = Math.min(90, 55 + stats.progressoes * 10);
      const indicadoresReais = [
        { n: 'Constância', v: pctConstancia },
        { n: 'Capacidade de progressão', v: capProgressao },
        { n: 'Capacidade de recuperação', v: capRecuperacao }
      ];
      indicadoresReais.forEach(function(i){
        indsHtml += '<div style="margin-bottom:12px;"><div style="display:flex;justify-content:space-between;font-size:12px;margin-bottom:4px;"><span style="color:var(--text-dim);">' + i.n + '</span><span style="font-weight:600;">' + i.v + '</span></div><div style="background:#221F18;border-radius:6px;height:6px;overflow:hidden;margin-bottom:5px;"><div style="width:' + i.v + '%;height:100%;background:linear-gradient(90deg,#4C3E25,#E8C58A);"></div></div><p style="font-size:11px;color:var(--text-faint);margin:0;">' + explicacaoIndicador(i.n, i.v) + (i.n === 'Capacidade de recuperação' && ajusteIdade.nota ? ' ' + ajusteIdade.nota : '') + '</p></div>';
      });
      const nutriStats = calcularNutricaoStats(NOME_ALUNA_LOGADA);
      if(nutriStats.temDados){
        [{ n: 'Potencial de hipertrofia', v: nutriStats.potencialHipertrofia }, { n: 'Resposta nutricional', v: nutriStats.respostaNutricional }, { n: 'Potencial de ganho de gordura', v: nutriStats.potencialGanhoGordura }, { n: 'Potencial de definição física', v: calcularPotencialDefinicao(NOME_ALUNA_LOGADA) }].forEach(function(i){
          indsHtml += '<div style="margin-bottom:12px;"><div style="display:flex;justify-content:space-between;font-size:12px;margin-bottom:4px;"><span style="color:var(--text-dim);">' + i.n + '</span><span style="font-weight:600;">' + i.v + '</span></div><div style="background:#221F18;border-radius:6px;height:6px;overflow:hidden;margin-bottom:5px;"><div style="width:' + i.v + '%;height:100%;background:linear-gradient(90deg,#4C3E25,#E8C58A);"></div></div><p style="font-size:11px;color:var(--text-faint);margin:0;">' + explicacaoIndicador(i.n, i.v) + '</p></div>';
        });
        if(nutriStats.retencaoHidricaEstimada > 0){
          indsHtml += '<div class="insight" style="margin-top:6px;"><p><b>Retenção hídrica estimada: ' + nutriStats.retencaoHidricaEstimada + '</b><br>Isso é água, não gordura, geralmente resolve em alguns dias voltando à hidratação normal. ' + (nutriStats.semanasComAlcool > 0 ? 'Parte disso pode estar ligado ao consumo de álcool relatado.' : '') + '</p></div>';
        }
      } else {
        ['Potencial de hipertrofia', 'Resposta nutricional'].forEach(function(nome){
          indsHtml += '<div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:10px;"><span style="font-size:12px;color:var(--text-dim);">' + nome + '</span><span style="font-size:11px;color:var(--text-faint);border:1px dashed var(--border-strong);padding:3px 8px;border-radius:8px;">Calibrando</span></div>';
        });
      }
    }
    const heroTexto = stats.temDados ? calcularProbabilidadeSucesso(NOME_ALUNA_LOGADA) + '%' : 'Calibrando, dia 1';
    el.innerHTML =
      '<p class="page-sub" style="margin-top:14px;">DNA MUSA</p>' +
      '<h1 class="page-title" style="margin-top:0;">Perfil inteligente da ' + NOME_ALUNA_LOGADA.split(' ')[0] + '</h1>' +
      '<div class="hero-card" style="cursor:default;">' +
        '<p class="hero-eyebrow">Probabilidade de sucesso</p>' +
        '<p class="ring-num" style="position:relative;font-size:18px;">' + heroTexto + '</p>' +
      '</div>' + indsHtml;
  } else if(type === 'evolucao'){
    const stats = calcularEstatisticasAluna(NOME_ALUNA_LOGADA);
    if(!stats.temDados){
      el.innerHTML =
        '<p class="page-sub" style="margin-top:14px;">DNA MUSA</p>' +
        '<h1 class="page-title" style="margin-top:0;">Evolução mensal</h1>' +
        '<div class="insight"><p>Ainda não há histórico, assim que você completar seus primeiros treinos, essa tela vai mostrar sua evolução mês a mês.</p></div>' +
        '<p class="section-label">Dias de treino por mês</p>' +
        '<div class="info-box"><p class="txt">Julho (mês atual): 0 treinos registrados até agora.</p></div>';
    } else {
      const prog = getProgressoAluna(NOME_ALUNA_LOGADA);
      const mes1 = [1,2,3,4,5,6].reduce(function(s,n){ return s + (prog.diasConcluidos[n] ? prog.diasConcluidos[n].length : 0); }, 0);
      const mes2 = [7,8].reduce(function(s,n){ return s + (prog.diasConcluidos[n] ? prog.diasConcluidos[n].length : 0); }, 0);
      el.innerHTML =
        '<p class="page-sub" style="margin-top:14px;">DNA MUSA</p>' +
        '<h1 class="page-title" style="margin-top:0;">Evolução mensal</h1>' +
        '<div class="insight"><p>Você já treinou ' + stats.totalConcluidos + ' dias desde que começou, ' + stats.progressoes + ' exercícios já tiveram aumento de carga.</p></div>' +
        '<p class="section-label">Dias de treino por mês (aprox.)</p>' +
        '<div class="info-box">' +
          '<div class="month-row"><span class="month-name">Mês atual (semanas 7-8)</span><div class="month-bar-wrap"><div class="month-bar" style="width:' + Math.round(mes2/10*100) + '%;"></div></div><span class="month-val">' + mes2 + '/10</span></div>' +
          '<div class="month-row"><span class="month-name">Mês anterior (semanas 1-6)</span><div class="month-bar-wrap"><div class="month-bar" style="width:' + Math.round(mes1/30*100) + '%;"></div></div><span class="month-val">' + mes1 + '/30</span></div>' +
        '</div>';
    }
  } else if(type === 'indicadores'){
    el.innerHTML =
      '<p class="page-sub" style="margin-top:14px;">DNA MUSA</p>' +
      '<h1 class="page-title" style="margin-top:0;">Indicadores</h1>' +
      '<div class="insight"><p>A explicação detalhada de cada indicador para a aluna ainda está em desenvolvimento, em breve cada métrica virá com uma leitura em linguagem simples.</p></div>';
  } else if(type === 'mobilidade'){
    const m = mobilidadeItens[arg];
    const embedMob = getEmbedUrl(m.video);
    el.innerHTML =
      (embedMob
        ? '<div class="video-block" style="margin-top:14px;"><iframe src="' + embedMob + '" title="' + m.n + '" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></div>' + ytFallback(m.video)
        : '<div class="video-block" style="margin-top:14px;"><i class="ti ti-player-play"></i></div>') +
      '<h1 class="page-title" style="margin-top:0;">' + m.n + '</h1>' +
      '<p class="page-sub">' + m.dur + (m.desvio ? ' · pode ser feito em casa' : '') + '</p>' +
      (m.desvio ? '<div class="info-box"><p class="lbl">O que identificamos</p><p class="txt">' + m.desvio + '</p><p class="lbl">O que estamos melhorando</p><p class="txt">' + m.foco + '</p></div>'
                : '<div class="info-box"><p class="txt">' + m.foco + '</p></div>');
  } else if(type === 'dia'){
    const d = dias[arg];
    if(detailDiaAtual !== arg){
      if(seriesConcluidasSalvasNoBanco && diaDasSeriesConcluidasSalvas === arg){
        // Tem checklist salvo do banco, e é exatamente o mesmo dia que ela tinha marcado antes de fechar o app
        seriesConcluidas = seriesConcluidasSalvasNoBanco;
      } else {
        seriesConcluidas = {};
      }
      seriesConcluidasSalvasNoBanco = null; // já usou (ou não servia), não aplica de novo à toa depois
    }
    detailDiaAtual = arg;
    let corpoTreino = '<div class="list-item"><span>Dia de descanso, aproveite para recuperar.</span></div>';
    let barraProgresso = '';
    if(d.tabata){
      corpoTreino = '<div class="info-box" style="margin-bottom:14px;">' +
        '<p class="lbl">Tabata · ' + d.nivel + '</p>' +
        '<p class="txt">' + d.trabalhoSeg + 's de trabalho, ' + d.descansoSeg + 's de descanso, ' + d.ciclos + ' ciclos por bloco · ~' + d.duracaoTotalMin + ' minutos no total</p>' +
        '<p class="txt" style="color:var(--text-faint);font-size:11px;">' + d.descricaoProtocolo + '</p>' +
      '</div>' +
      d.blocos.map(function(bloco, bi){ return '<div class="list-item"><span>Bloco ' + (bi+1) + ': ' + bloco.nome + '</span></div>'; }).join('') +
      '<button class="btn-gold" style="margin-top:14px;" onclick="iniciarTabata(' + JSON.stringify(d).replace(/"/g,'&quot;') + ')"><i class="ti ti-player-play" style="margin-right:6px;"></i>Iniciar Tabata</button>';
    } else if(!d.descanso){
      const alunaAtual = alunasPersonal.find(function(a){ return a.nome === NOME_ALUNA_LOGADA; });
      const prog = alunaAtual ? getProgressoAluna(alunaAtual.nome) : null;

      // Barra de progresso segmentada: 1 segmento por exercício do dia
      const totalExerciciosDia = d.ex.length;
      barraProgresso = '<div style="display:flex;gap:3px;margin:4px 0 14px;" id="barra-progresso-dia">' +
        d.ex.map(function(_, idx){ return '<div class="segmento-progresso" data-idx="' + idx + '" style="flex:1;height:5px;border-radius:3px;background:var(--border);transition:background .2s;"></div>'; }).join('') +
        '</div>';

      corpoTreino = '<div class="badge">' + faseAtual + '</div>' +
        '<div class="list-item" style="justify-content:space-between;"><span id="cronometro-display" style="font-family:\'Playfair Display\',serif;font-size:18px;font-weight:600;">' + String(Math.floor(cronometroSegundos/60)).padStart(2,'0') + ':' + String(cronometroSegundos%60).padStart(2,'0') + '</span><button class="btn-gold" style="width:auto;margin:0;padding:8px 16px;" id="cronometro-btn" onclick="alternarCronometro()">' + (cronometroRodando ? 'Pausar' : (cronometroSegundos > 0 ? 'Retomar' : 'Iniciar treino')) + '</button></div>' +
        '<div id="toast-celebracao-area"></div>' +
        renderBlocoPosturalDia(alunaAtual) +
        '<p class="section-label">Treino do dia</p>' +
        barraProgresso;
      d.ex.forEach(function(linha, j){
        if(linha.indexOf('|||') !== -1){
          corpoTreino += renderCardBiset(linha, j, prog);
          return;
        }
        const partes = linha.split(' · ');
        const nomeEx = partes[0];
        let notaSemanaAnterior = '';
        if(prog && prog.historico[nomeEx] && prog.historico[nomeEx].length){
          const ultimo = prog.historico[nomeEx][prog.historico[nomeEx].length - 1];
          notaSemanaAnterior = '<p style="font-size:11px;color:var(--gold-soft);margin:4px 0 8px;">Semana ' + ultimo.semana + ': ' + ultimo.carga + 'kg × ' + ultimo.reps + ' reps · ' + ultimo.sugestao.texto + ' → sugestão hoje: ' + ultimo.sugestao.valor + 'kg</p>';
        }
        const exBanco = buscarExercicioNoBanco(nomeEx);
        const repsMatch = (partes[1] || '').match(/x(\d+)/);
        const repsAlvo = repsMatch ? parseInt(repsMatch[1], 10) : null;
        const descanso = calcularDescansoPorReps(repsAlvo);
        const numSeries = repsMatch ? parseInt(partes[1].split('x')[0], 10) : 0;
        if(!seriesConcluidas[j]) seriesConcluidas[j] = new Array(numSeries).fill(false);
        let pillsSeries = '<div style="display:flex;gap:6px;flex-wrap:wrap;margin:2px 0 8px;">';
        for(let s = 0; s < numSeries; s++){
          const feita = seriesConcluidas[j][s];
          pillsSeries += '<span class="chip" id="serie-pill-' + j + '-' + s + '" style="padding:10px 18px;font-size:13px;font-weight:600;cursor:pointer;min-height:20px;' + (feita ? 'background:var(--success-soft);border-color:var(--success);color:var(--success);' : '') + '" onclick="concluirSerie(' + j + ',' + s + ',' + descansoParaSegundos(descanso || '60s') + ')">' + (feita ? '✓ ' : '') + 'Série ' + (s+1) + '</span>';
        }
        pillsSeries += '</div><span id="descanso-display-' + j + '" style="font-size:13px;font-weight:600;color:var(--gold-soft);"></span>';

        const embedVideo = exBanco ? getEmbedUrl(exBanco.video) : null;
        const playerVideo = embedVideo
          ? '<div class="video-block" style="margin-bottom:8px;"><iframe src="' + embedVideo + '" title="' + nomeEx + '" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></div>'
          : '';

        const precisaTecnica = precisaAprovacaoTecnica(alunaAtual, nomeEx);
        const semanaPar = prog ? (prog.semana % 2 === 0) : false;
        const mostrarAvisoTecnica = precisaTecnica === true || (precisaTecnica === 'periodica' && semanaPar);
        let avisoTecnica = '';
        if(mostrarAvisoTecnica){
          const jaSolicitado = alunaAtual.tecnicaAprovada && alunaAtual.tecnicaAprovada[nomeEx] === 'pendente';
          avisoTecnica = jaSolicitado
            ? '<p style="font-size:11px;color:var(--gold-soft);margin:2px 0 8px;">📹 Vídeo enviado, aguardando seu personal analisar.</p>'
            : '<p style="font-size:11px;color:var(--gold-soft);margin:2px 0 8px;cursor:pointer;" onclick="pedirVideoTecnicaAluna(\'' + nomeEx.replace(/'/g,"\\'") + '\')">📹 Grave um vídeo desse exercício e envie pro seu personal analisar a técnica</p>';
        }

        const alternativaHtml = '<p style="font-size:11px;color:var(--text-faint);margin:2px 0 8px;cursor:pointer;" onclick="mostrarAlternativaEmergencia(' + j + ',\'' + nomeEx.replace(/'/g,"\\'") + '\')">Equipamento ocupado? Ver alternativa</p><div id="alt-emergencia-' + j + '" style="display:none;margin-bottom:8px;"></div>';

        const metodoDoExercicio = d.metodos && d.metodos[j];
        const cardMetodo = metodoDoExercicio
          ? '<div class="info-box" style="margin-bottom:8px;">' +
              '<p class="lbl" style="margin-bottom:4px;">Sobre o método: ' + metodoDoExercicio + '</p>' +
              '<p class="txt" style="margin-bottom:8px;">' + (descricoesMetodo[metodoDoExercicio] || '') + '</p>' +
              '<div id="metodo-pergunta-' + j + '"><p class="txt" style="font-size:12px;margin-bottom:6px;">Você entendeu como funciona esse método?</p>' +
                '<div style="display:flex;gap:8px;">' +
                  '<span class="chip" style="cursor:pointer;" onclick="responderEntendimentoMetodo(' + j + ',true)">Sim, entendi</span>' +
                  '<span class="chip" style="cursor:pointer;" onclick="responderEntendimentoMetodo(' + j + ',false)">Não, tenho dúvida</span>' +
                '</div></div>' +
            '</div>'
          : '';

        const conteudoExpandido = playerVideo +
          cardMetodo +
          (descanso ? pillsSeries : '') +
          avisoTecnica +
          alternativaHtml +
          notaSemanaAnterior +
          '<div style="display:flex;gap:8px;">' +
            '<input class="form-input" data-carga="' + j + '" type="number" placeholder="Carga (kg)" style="flex:1;">' +
            '<input class="form-input" data-reps="' + j + '" type="number" placeholder="Reps · 1ª série" style="flex:1;">' +
          '</div>';

        corpoTreino += '<div class="list-item" style="flex-direction:column;align-items:stretch;gap:0;padding:0;overflow:hidden;">' +
          '<div style="display:flex;">' +
            '<div style="flex:1;display:flex;justify-content:space-between;align-items:center;cursor:pointer;padding:14px 12px;" onclick="alternarExercicioExpandido(' + j + ')">' +
              '<span>' + nomeEx + (d.metodos && d.metodos[j] ? ' <span class="tag" style="background:var(--gold-soft);color:#1A1409;">' + d.metodos[j] + '</span>' : '') + '</span>' +
              '<span class="tag">' + (partes[1] || '') + (descanso ? ' · ⏱ ' + descanso : '') + '</span>' +
            '</div>' +
            '<div style="width:14px;display:flex;align-items:center;justify-content:center;cursor:pointer;background:var(--card-2);" title="Ver opções parecidas" onclick="event.stopPropagation();alternarSubstituicoesInline(' + j + ',\'' + nomeEx.replace(/'/g,"\\'") + '\')">' +
              '<div style="width:3px;height:24px;border-radius:2px;background:var(--gold-soft);"></div>' +
            '</div>' +
          '</div>' +
          '<div id="subs-inline-' + j + '" style="display:none;padding:0 12px;"></div>' +
          '<div id="ex-expandido-' + j + '" style="display:none;padding:0 12px 12px;">' + conteudoExpandido + '</div>' +
        '</div>';
      });
      corpoTreino += '<button class="btn-gold" style="margin-top:10px;" onclick="registrarTreinoDia(' + arg + ')">Registrar treino de hoje</button>' +
        '<div id="registro-confirmacao" style="margin-top:10px;"></div>';
    }
    el.innerHTML =
      '<p class="page-sub" style="margin-top:14px;">' + d.n + '-feira' + (d.hoje ? ' · hoje' : '') + '</p>' +
      '<h1 class="page-title" style="margin-top:0;">' + d.foco + '</h1>' +
      corpoTreino;
    if(!d.descanso) atualizarBarraProgressoDia(d);
  } else if(type === 'conteudo'){
    const c = arg;
    if(c.aulas){
      abrirCurso(c);
      setActive('detail');
      document.getElementById('backlabel').textContent = 'Voltar';
      return;
    }
    const embed = getEmbedUrl(c.video);
    el.innerHTML =
      '<p class="page-sub" style="margin-top:14px;">' + c.cat + '</p>' +
      '<h1 class="page-title" style="margin-top:0;">' + c.n + '</h1>' +
      (c.locked
        ? '<div class="video-block"><i class="ti ti-lock" style="font-size:32px;color:var(--gold-soft);"></i></div>'
        : (embed
          ? '<div class="video-block"><iframe src="' + embed + '" title="' + c.n + '" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></div>' + ytFallback(c.video)
          : '<div class="video-block"><i class="ti ti-player-play"></i></div>')) +
      '<div class="info-box"><p class="txt">' + c.desc + '</p></div>' +
      (c.locked
        ? '<div class="list-item"><span><i class="ti ti-lock" style="font-size:13px;vertical-align:-2px;margin-right:6px;color:var(--gold-soft);"></i>Conteúdo bloqueado</span><span class="tag">' + (c.preco || 'Desbloquear') + '</span></div>' +
          (c.preco ? '<p class="page-sub" style="margin-top:8px;">Pagamentos ainda não estão ativos na plataforma, isso é só uma prévia de como vai aparecer.</p>' : '')
        : '');
  } else {
    el.innerHTML =
      '<div class="video-block" style="margin-top:14px;"><i class="ti ti-player-play"></i></div>' +
      '<h1 class="page-title" style="margin-top:0;">Conteúdo em vídeo</h1>' +
      '<p class="page-sub">Player entra aqui na versão final do app.</p>';
  }
  setActive('detail');
  document.getElementById('backlabel').textContent = 'Voltar';
  aplicarTransicaoSuave('detail-content');
}

function responderEntendimentoMetodo(idx, entendeu){
  const container = document.getElementById('metodo-pergunta-' + idx);
  if(!container) return;
  if(entendeu){
    container.innerHTML = '<p class="txt" style="font-size:12px;color:var(--gold-soft);">✓ Que bom! Qualquer dúvida durante o treino é só chamar a Sol.</p>';
  } else {
    const alunaAtual = alunasPersonal.find(function(a){ return a.nome === NOME_ALUNA_LOGADA; });
    if(alunaAtual){
      if(!alunaAtual.duvidasSinalizadas) alunaAtual.duvidasSinalizadas = [];
      const nomeEx = dias[detailDiaAtual].ex[idx].split(' · ')[0];
      const metodoNome = dias[detailDiaAtual].metodos && dias[detailDiaAtual].metodos[idx];
      alunaAtual.duvidasSinalizadas.push({ pergunta: 'Não entendeu o método "' + metodoNome + '" no exercício ' + nomeEx, respostaSol: 'Sinalizado automaticamente, sem resposta da Sol ainda. Revise com ela na próxima conversa.', resolvida: false, data: new Date().toISOString() });
    }
    container.innerHTML = '<p class="txt" style="font-size:12px;color:var(--gold-soft);">Sem problema! Já avisei seu personal pra explicar melhor na próxima conversa. Por enquanto, pode fazer o exercício sem o método extra, só as séries normais.</p>';
  }
}

function pedirVideoTecnicaAluna(nomeEx){
  const alunaAtual = alunasPersonal.find(function(a){ return a.nome === NOME_ALUNA_LOGADA; });
  if(!alunaAtual) return;
  solicitarVideoTecnica(alunaAtual.nome, nomeEx);
  openDetail('dia', detailDiaAtual);
}

function mostrarAlternativaEmergencia(idx, nomeEx){
  const container = document.getElementById('alt-emergencia-' + idx);
  if(!container) return;
  if(container.style.display === 'none' || !container.innerHTML){
    const alt = sugerirAlternativaEmergencia(nomeEx);
    let html = '<div class="info-box">';
    if(alt.nivel1_halteres.length){
      html += '<p class="lbl">Opção com halteres (mais confiável)</p>';
      alt.nivel1_halteres.forEach(function(op){ html += '<p class="txt">• ' + op + '</p>'; });
    } else {
      html += '<p class="txt">' + alt.nivel2_backup + '</p>';
    }
    html += '<p class="txt" style="color:var(--text-faint);font-size:11px;margin-top:8px;">Se nada disso estiver disponível: ' + alt.nivel3_pular + '</p>';
    html += '</div>';
    container.innerHTML = html;
    container.style.display = 'block';
  } else {
    container.style.display = 'none';
  }
}

function renderCardBiset(linha, j, prog){
  const partesMetodo = linha.split('|||');
  const metodoNome = partesMetodo[0].trim(); // ex: "Bi-set"
  const exercicios = partesMetodo.slice(1); // ["Nome1 · SxR1", "Nome2 · SxR2", ...]

  const descricoes = {
    'Bi-set': 'Bi-set: execute os dois exercícios em sequência, sem descanso entre eles, descanse só depois do segundo.',
    'Tri-set': 'Tri-set: execute os três exercícios em sequência, sem descanso entre eles, descanse só depois do último.'
  };

  let html = '<div class="list-item" style="flex-direction:column;align-items:stretch;gap:6px;border:1px solid var(--gold-deep);">' +
    '<span class="badge" style="align-self:flex-start;">' + metodoNome + '</span>' +
    '<p style="font-size:11px;color:var(--text-faint);margin:0 0 4px;">' + (descricoes[metodoNome] || '') + '</p>';

  exercicios.forEach(function(exLinha, sub){
    const partes = exLinha.trim().split(' · ');
    const nomeEx = partes[0];
    const subId = j + '-' + sub;
    const seriesMatchBiset = (partes[1] || '').match(/^(\d+)x/);
    const numSeriesBiset = seriesMatchBiset ? parseInt(seriesMatchBiset[1], 10) : 0;
    const repsMatchBiset = (partes[1] || '').match(/x(\d+)/);
    const repsAlvoBiset = repsMatchBiset ? parseInt(repsMatchBiset[1], 10) : null;
    const descansoBiset = calcularDescansoPorReps(repsAlvoBiset);
    const ehUltimoDoPar = sub === exercicios.length - 1;

    if(!seriesConcluidas[subId]) seriesConcluidas[subId] = new Array(numSeriesBiset).fill(false);
    let pillsBiset = '<div style="display:flex;gap:6px;flex-wrap:wrap;margin:4px 0 2px;">';
    for(let s = 0; s < numSeriesBiset; s++){
      const feita = seriesConcluidas[subId][s];
      pillsBiset += '<span class="chip" id="serie-pill-' + subId + '-' + s + '" style="padding:9px 16px;font-size:12px;font-weight:600;cursor:pointer;' + (feita ? 'background:var(--success-soft);border-color:var(--success);color:var(--success);' : '') + '" onclick="concluirSerieBiset(\'' + subId + '\',' + s + ',' + ehUltimoDoPar + ',' + descansoParaSegundos(descansoBiset || '60s') + ')">' + (feita ? '✓ ' : '') + 'Série ' + (s+1) + '</span>';
    }
    pillsBiset += '</div>';

    let nota = '';
    if(prog && prog.historico[nomeEx] && prog.historico[nomeEx].length){
      const ultimo = prog.historico[nomeEx][prog.historico[nomeEx].length - 1];
      nota = '<p style="font-size:11px;color:var(--gold-soft);margin:2px 0 6px;">Semana ' + ultimo.semana + ': ' + ultimo.carga + 'kg × ' + ultimo.reps + ' reps → sugestão: ' + ultimo.sugestao.valor + 'kg</p>';
    }
    html += '<div style="border-top:1px dashed var(--border);padding-top:8px;margin-top:4px;">' +
      '<div style="display:flex;justify-content:space-between;"><span>' + nomeEx + '</span><span class="tag">' + (partes[1] || '') + (ehUltimoDoPar && descansoBiset ? ' · ⏱ ' + descansoBiset + ' após o par' : ' · sem descanso →') + '</span></div>' +
      pillsBiset +
      (ehUltimoDoPar ? '<span id="descanso-display-' + subId + '" style="font-size:12px;font-weight:600;color:var(--gold-soft);"></span>' : '') +
      '<p style="font-size:12px;color:var(--gold-soft);margin:2px 0 6px;cursor:pointer;" onclick="toggleVideoExercicioDia(\'' + subId + '\')"><i class="ti ti-player-play" style="font-size:12px;vertical-align:-1px;margin-right:4px;"></i>Ver execução</p>' +
      '<div id="video-ex-dia-' + subId + '" style="display:none;margin-bottom:6px;"></div>' +
      nota +
      '<div style="display:flex;gap:8px;">' +
        '<input class="form-input" data-carga="' + subId + '" type="number" placeholder="Carga (kg)" style="flex:1;">' +
        '<input class="form-input" data-reps="' + subId + '" type="number" placeholder="Reps · 1ª série" style="flex:1;">' +
      '</div>' +
    '</div>';
  });

  html += '</div>';
  return html;
}

function renderBlocoPosturalDia(alunaAtual){
  if(!alunaAtual || !alunaAtual.desviosPosturaisConfirmados || alunaAtual.desviosPosturaisConfirmados.length === 0) return '';
  const bloco = obterBlocoPostural(alunaAtual.desviosPosturaisConfirmados);
  if(bloco.length === 0) return '';
  let html = '<p class="section-label">Mobilidade e correção postural</p>' +
    '<p class="page-sub" style="margin-top:-6px;">Faça antes do treino principal de hoje</p>';
  bloco.forEach(function(ex, i){
    const idxPostural = 'postural-' + i;
    const seriesMatch = ex.volume.match(/^(\d+)x/);
    const numSeries = seriesMatch ? parseInt(seriesMatch[1], 10) : 0;
    if(!seriesConcluidas[idxPostural]) seriesConcluidas[idxPostural] = new Array(numSeries).fill(false);
    const descansoSegundos = descansoParaSegundos(ex.descanso || '30s');

    let pills = '<div style="display:flex;gap:6px;flex-wrap:wrap;margin:4px 0 2px;">';
    for(let s = 0; s < numSeries; s++){
      const feita = seriesConcluidas[idxPostural][s];
      pills += '<span class="chip" id="serie-pill-' + idxPostural + '-' + s + '" style="padding:9px 16px;font-size:12px;font-weight:600;cursor:pointer;' + (feita ? 'background:var(--success-soft);border-color:var(--success);color:var(--success);' : '') + '" onclick="concluirSerie(\'' + idxPostural + '\',' + s + ',' + descansoSegundos + ')">' + (feita ? '✓ ' : '') + 'Série ' + (s+1) + '</span>';
    }
    pills += '</div>';

    const embedVideo = (function(){
      let exBanco = buscarExercicioNoBanco(ex.nome);
      if(!exBanco) exBanco = mobilidadeBanco.find(function(m){ return m.nome.toUpperCase() === ex.nome.toUpperCase(); });
      return exBanco ? getEmbedUrl(exBanco.video) : null;
    })();
    const playerVideo = embedVideo
      ? '<div class="video-block" style="margin-bottom:8px;"><iframe src="' + embedVideo + '" title="' + ex.nome + '" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></div>'
      : '<div class="video-block" style="flex-direction:column;gap:6px;margin-bottom:8px;"><i class="ti ti-video-off" style="font-size:26px;color:var(--text-faint);"></i><p style="font-size:12px;color:var(--text-faint);margin:0;text-align:center;padding:0 20px;">Este vídeo ainda não está disponível, o link será atualizado em breve.</p></div>';

    html += '<div class="list-item" style="flex-direction:column;align-items:stretch;gap:0;padding:0;overflow:hidden;">' +
      '<div style="display:flex;justify-content:space-between;align-items:center;cursor:pointer;padding:14px 12px;" onclick="alternarExercicioExpandido(\'' + idxPostural + '\')">' +
        '<span>' + ex.nome + '</span>' +
        '<span class="tag">' + ex.volume + ' · ⏱ ' + ex.descanso + '</span>' +
      '</div>' +
      '<div id="ex-expandido-' + idxPostural + '" style="display:none;padding:0 12px 12px;">' +
        playerVideo +
        pills +
        '<span id="descanso-display-' + idxPostural + '" style="font-size:12px;font-weight:600;color:var(--gold-soft);"></span>' +
      '</div>' +
    '</div>';
  });
  return html;
}

function toggleVideoPostural(idx, nomeExercicio){
  const container = document.getElementById('video-postural-' + idx);
  if(!container) return;
  if(container.style.display === 'none' || !container.innerHTML){
    let exBanco = buscarExercicioNoBanco(nomeExercicio);
    if(!exBanco) exBanco = mobilidadeBanco.find(function(m){ return m.nome.toUpperCase() === nomeExercicio.toUpperCase(); });
    const embed = exBanco ? getEmbedUrl(exBanco.video) : null;
    container.innerHTML = embed
      ? '<div class="video-block"><iframe src="' + embed + '" title="' + nomeExercicio + '" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></div>' + ytFallback(exBanco.video)
      : '<div class="video-block" style="flex-direction:column;gap:6px;"><i class="ti ti-video-off" style="font-size:26px;color:var(--text-faint);"></i><p style="font-size:12px;color:var(--text-faint);margin:0;text-align:center;padding:0 20px;">Este vídeo ainda não está disponível, o link será atualizado em breve.</p></div>';
    container.style.display = 'block';
  } else {
    container.style.display = 'none';
  }
}

function toggleVideoExercicioDia(idx){
  const container = document.getElementById('video-ex-dia-' + idx);
  if(!container) return;
  if(container.style.display === 'none' || !container.innerHTML){
    const d = dias[detailDiaAtual];
    let nomeEx;
    const idxStr = String(idx);
    if(idxStr.indexOf('-') !== -1){
      const partesIdx = idxStr.split('-');
      const linhaBiset = d.ex[parseInt(partesIdx[0], 10)];
      const exercicios = linhaBiset.split('|||').slice(1);
      nomeEx = exercicios[parseInt(partesIdx[1], 10)].trim().split(' · ')[0];
    } else {
      nomeEx = d.ex[idx].split(' · ')[0];
    }
    const exBanco = buscarExercicioNoBanco(nomeEx);
    const embed = exBanco ? getEmbedUrl(exBanco.video) : '';
    container.innerHTML = embed
      ? '<div class="video-block"><iframe src="' + embed + '" title="' + nomeEx + '" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></div>' + ytFallback(exBanco.video)
      : '<div class="video-block" style="flex-direction:column;gap:6px;"><i class="ti ti-video-off" style="font-size:26px;color:var(--text-faint);"></i><p style="font-size:12px;color:var(--text-faint);margin:0;text-align:center;padding:0 20px;">Este vídeo ainda não está disponível, o link será atualizado em breve.</p></div>';
    container.style.display = 'block';
  } else {
    container.style.display = 'none';
  }
}

function renderPerguntaFeedbackTreino(diaIndex){
  const d = dias[diaIndex];
  let opcoesExercicios = '<option value="">Selecione...</option>';
  d.ex.forEach(function(linha){
    if(linha.indexOf('|||') !== -1){
      linha.split('|||').slice(1).forEach(function(sub){
        const nome = sub.trim().split(' · ')[0];
        opcoesExercicios += '<option value="' + nome + '">' + nome + '</option>';
      });
    } else {
      const nome = linha.split(' · ')[0];
      opcoesExercicios += '<option value="' + nome + '">' + nome + '</option>';
    }
  });

  return '<div class="info-box" id="area-feedback" style="margin-top:10px;">' +
    '<p class="lbl">Como foi o treino de hoje?</p>' +
    '<div class="form-group"><label class="form-label">Sentiu desconforto em algum exercício?</label>' +
      '<select class="form-select" id="feedback-desconforto" onchange="alternarCampoExercicioFeedback()"><option>Não</option><option>Sim</option></select></div>' +
    '<div class="form-group" id="feedback-exercicio-group" style="display:none;">' +
      '<label class="form-label">Qual exercício?</label>' +
      '<select class="form-select" id="feedback-exercicio-desconforto">' + opcoesExercicios + '</select>' +
    '</div>' +
    '<div class="form-group" id="feedback-escala-group" style="display:none;"><label class="form-label">Numa escala de 0 a 10, qual a intensidade do desconforto?</label><input class="form-input" id="feedback-escala-desconforto" type="number" min="0" max="10" placeholder="0-10"></div>' +
    '<div class="form-group"><label class="form-label">E a intensidade geral do treino, de 0 a 10?</label><input class="form-input" id="feedback-intensidade" type="number" min="0" max="10" placeholder="0-10"></div>' +
    '<button class="btn-gold" onclick="registrarFeedbackTreino(' + diaIndex + ')">Enviar</button>' +
    '</div>';
}

function alternarCampoExercicioFeedback(){
  const val = document.getElementById('feedback-desconforto').value;
  document.getElementById('feedback-exercicio-group').style.display = val === 'Sim' ? 'block' : 'none';
  document.getElementById('feedback-escala-group').style.display = val === 'Sim' ? 'block' : 'none';
}

function registrarFeedbackTreino(diaIndex){
  const alunaAtual = alunasPersonal.find(function(a){ return a.nome === NOME_ALUNA_LOGADA; });
  if(!alunaAtual) return;
  const prog = getProgressoAluna(alunaAtual.nome);
  const desconforto = document.getElementById('feedback-desconforto').value === 'Sim';
  const exercicioDesconforto = desconforto ? document.getElementById('feedback-exercicio-desconforto').value : null;
  const escalaDesconforto = desconforto ? parseInt(document.getElementById('feedback-escala-desconforto').value, 10) : null;
  const intensidadeInput = parseInt(document.getElementById('feedback-intensidade').value, 10);

  if(!prog.feedbackTreino) prog.feedbackTreino = [];
  prog.feedbackTreino.push({
    semana: prog.semana,
    dia: dias[diaIndex].n,
    desconforto: desconforto,
    exercicio: exercicioDesconforto,
    escalaDesconforto: escalaDesconforto,
    intensidade: isNaN(intensidadeInput) ? null : intensidadeInput
  });

  let msg = 'Obrigada pelo feedback! Isso ajuda a calibrar seus próximos treinos.';

  // EXCEÇÃO: desconforto relatado num exercício específico dispara pedido de vídeo NA HORA,
  // sem esperar a regra de semana sim/semana não
  if(desconforto && exercicioDesconforto){
    solicitarVideoTecnica(alunaAtual.nome, exercicioDesconforto);
    msg += ' Como você relatou desconforto no ' + exercicioDesconforto + ', já pedimos um vídeo desse exercício pra revisar a técnica com você.';
  }

  const el = document.getElementById('area-feedback');
  if(el) el.innerHTML = '<p class="txt">' + msg + '</p>';
}

function registrarTreinoDia(diaIndex){
  try {
    const d = dias[diaIndex];
    const alunaAtual = alunasPersonal.find(function(a){ return a.nome === NOME_ALUNA_LOGADA; });
    if(!alunaAtual) return;
    const prog = getProgressoAluna(alunaAtual.nome);
    let registrados = 0;
    let houveReducao = false;
    d.ex.forEach(function(linha, j){
      let nomesEIndices = [{ nome: linha.split(' · ')[0], idx: String(j) }];
      if(linha.indexOf('|||') !== -1){
        const exercicios = linha.split('|||').slice(1);
        nomesEIndices = exercicios.map(function(exLinha, sub){
          return { nome: exLinha.trim().split(' · ')[0], idx: j + '-' + sub };
        });
      }
      nomesEIndices.forEach(function(item){
        const cargaEl = document.querySelector('[data-carga="' + item.idx + '"]');
        const repsEl = document.querySelector('[data-reps="' + item.idx + '"]');
        if(!cargaEl || !repsEl) return;
        const carga = parseFloat(cargaEl.value);
        const reps = parseInt(repsEl.value, 10);
        if(isNaN(carga) || isNaN(reps)) return;
        if(!prog.historico[item.nome]) prog.historico[item.nome] = [];
        const sugestao = sugerirAjusteCarga(carga, reps);
        prog.historico[item.nome].push({ semana: prog.semana, carga: carga, reps: reps, sugestao: sugestao });
        if(sugestao.texto.indexOf('edução') !== -1) houveReducao = true;
        registrados++;
      });
    });

    let resultadoSemana = null;
    if(registrados > 0){
      if(!prog.diasConcluidos[prog.semana]) prog.diasConcluidos[prog.semana] = [];
      if(prog.diasConcluidos[prog.semana].indexOf(d.n) === -1) prog.diasConcluidos[prog.semana].push(d.n);
      if(!prog.horariosTreino) prog.horariosTreino = [];
      const agora = new Date();
      prog.horariosTreino.push({ dia: d.n, hora: agora.getHours(), minuto: agora.getMinutes(), data: agora.toISOString() });
      resultadoSemana = checarConclusaoSemana(alunaAtual.nome);
    }

    if(registrados > 0) salvarProgressoNoSupabase(alunaAtual.nome);

    openDetail('dia', diaIndex);
    const conf = document.getElementById('registro-confirmacao');
    if(registrados === 0){
      if(conf) conf.innerHTML = '<div class="insight"><p>Preencha ao menos um exercício com carga e repetições pra registrar.</p></div>';
      return;
    }

    let msg = registrados + ' exercício(s) registrado(s).';
    if(resultadoSemana && resultadoSemana.avancou){
      msg += ' Semana concluída (' + resultadoSemana.total + '/' + resultadoSemana.total + '), suas progressões de carga já estão calculadas pra próxima semana! 🎉';
    } else if(resultadoSemana){
      msg += ' ' + resultadoSemana.concluidos + ' de ' + resultadoSemana.total + ' treinos concluídos essa semana.';
    }
    if(conf){
      conf.innerHTML = '<div class="insight"><p>' + msg + '</p></div>';
      conf.innerHTML += renderPerguntaFeedbackTreino(diaIndex);

      if(resultadoSemana && resultadoSemana.avancou){
        const semanaFechada = prog.semana - 1;
        if(!prog.nutricao || !prog.nutricao[semanaFechada]){
          conf.innerHTML += renderPerguntaNutricao(semanaFechada);
        }
        const mesPendente = verificarCheckinPeso(alunaAtual.nome);
        if(mesPendente !== null){
          conf.innerHTML += renderPerguntaPeso(mesPendente);
        }
      }

      if(houveReducao && !alunaAtual.cicloPerguntado && !alunaAtual.cicloInfo){
        conf.innerHTML += renderPerguntaCiclo();
      }
    }
  } catch(erroInesperado){
    console.error('[registrarTreinoDia] erro inesperado, mas capturado, nada quebrou:', erroInesperado);
    const conf = document.getElementById('registro-confirmacao');
    if(conf) conf.innerHTML = '<div class="insight"><p>Não consegui confirmar agora, mas o que você já preencheu não foi perdido. Tenta de novo em alguns segundos.</p></div>';
  }
}

function renderPerguntaCiclo(){
  return '<div class="info-box" id="area-ciclo" style="margin-top:10px;">' +
    '<p class="lbl">Notamos algo</p>' +
    '<p class="txt">Notamos um desempenho um pouco mais baixo hoje, e tudo bem, isso acontece. Sabemos que o período pode influenciar a força em alguns momentos. Se quiser, pode nos contar há quantos dias foi sua última menstruação? Isso ajuda a deixar nossas análises mais precisas pra você.</p>' +
    '<div class="form-group"><input class="form-input" id="ciclo-dias" type="number" placeholder="Ex: 20 dias atrás"></div>' +
    '<div class="form-group"><label class="form-label">Usa algum medicamento/anticoncepcional?</label><select class="form-select" id="ciclo-medicamento"><option>Não</option><option>Sim</option></select></div>' +
    '<button class="btn-gold" style="background:var(--card-2);color:var(--gold-soft);border:1px solid var(--border);" onclick="registrarCiclo()">Enviar</button>' +
    '<button class="btn-gold" onclick="pularCiclo()">Prefiro não responder</button>' +
    '</div>';
}

function registrarCiclo(){
  const alunaAtual = alunasPersonal.find(function(a){ return a.nome === NOME_ALUNA_LOGADA; });
  const dias_atras = parseInt(document.getElementById('ciclo-dias').value, 10);
  const medicamento = document.getElementById('ciclo-medicamento').value;
  alunaAtual.cicloPerguntado = true;
  if(!isNaN(dias_atras)){
    const prog = getProgressoAluna(alunaAtual.nome);
    const semanasAteProximoPeriodo = Math.round((28 - dias_atras) / 7);
    alunaAtual.cicloInfo = { diasAtras: dias_atras, medicamento: medicamento, semanaEstimadaProximoPeriodo: prog.semana + semanasAteProximoPeriodo };
  }
  const elCiclo = document.getElementById('area-ciclo');
  if(elCiclo) elCiclo.innerHTML = '<p class="txt">Obrigada por compartilhar, isso vai deixar suas análises mais precisas, sem afetar nada além disso.</p>';
  salvarPerfilAlunaNoSupabase(alunaAtual.nome);
}

function pularCiclo(){
  const alunaAtual = alunasPersonal.find(function(a){ return a.nome === NOME_ALUNA_LOGADA; });
  alunaAtual.cicloPerguntado = true;
  salvarPerfilAlunaNoSupabase(alunaAtual.nome);
  const elCiclo2 = document.getElementById('area-ciclo');
  if(elCiclo2) elCiclo2.innerHTML = '<p class="txt">Sem problemas, pode responder outra hora, se quiser.</p>';
}

function simularHistorico40Dias(){
  const nome = NOME_ALUNA_LOGADA;
  const a = alunasPersonal.find(function(x){ return x.nome === nome; });
  if(!a) return;

  progressoesPorAluna[nome] = { semana: 1, historico: {}, diasConcluidos: {}, substituicoes: [] };
  const prog = progressoesPorAluna[nome];

  const semanas = [
    { dias: ['Segunda','Terça','Quarta','Quinta','Sexta'] },
    { dias: ['Segunda','Terça','Quarta','Quinta','Sexta'] },
    { dias: ['Segunda','Terça','Quarta','Quinta'] },
    { dias: ['Segunda','Terça','Quarta','Quinta','Sexta'] },
    { dias: ['Segunda','Quarta','Quinta'] },
    { dias: ['Terça','Quarta','Quinta','Sexta'] },
    { dias: ['Segunda','Terça','Quarta','Sexta'] },
    { dias: ['Segunda','Terça'] }
  ];

  const legPress = [
    {carga:40, reps:13}, {carga:43, reps:11}, {carga:43, reps:10},
    {carga:43, reps:13}, {carga:46.5, reps:10}, {carga:46.5, reps:11}, {carga:46.5, reps:10}
  ];
  const agachamentoHack = [
    {carga:20, reps:10}, null, {carga:20, reps:11}, null, null, {carga:20, reps:11}, {carga:20, reps:10}
  ];
  const cadeiraFlexora = [
    null, {carga:15, reps:10}, null, null, {carga:15, reps:4}, null, {carga:13.5, reps:9}
  ];

  function registrarSemana(nomeEx, listaSemanas){
    listaSemanas.forEach(function(dado, idx){
      if(!dado) return;
      const semana = idx + 1;
      const sugestao = sugerirAjusteCarga(dado.carga, dado.reps);
      if(!prog.historico[nomeEx]) prog.historico[nomeEx] = [];
      prog.historico[nomeEx].push({ semana: semana, carga: dado.carga, reps: dado.reps, sugestao: sugestao });
    });
  }
  registrarSemana('Leg Press 45', legPress);
  registrarSemana('Agachamento Hack', agachamentoHack);
  registrarSemana('Cadeira flexora', cadeiraFlexora);

  semanas.forEach(function(s, idx){
    prog.diasConcluidos[idx + 1] = s.dias.slice();
  });
  prog.semana = 8;

  // Simula horário real de cada treino, com pequena variação em torno de um padrão (6h30 da manhã)
  prog.horariosTreino = [];
  const variacoesMinuto = [0, 12, -8, 5, 20, -15, 3, 18, -5, 10];
  let contadorHorario = 0;
  semanas.forEach(function(s){
    s.dias.forEach(function(nomeDia){
      const variacao = variacoesMinuto[contadorHorario % variacoesMinuto.length];
      const minutoBase = 30 + variacao;
      const hora = minutoBase < 0 ? 5 : (minutoBase >= 60 ? 7 : 6);
      const minuto = ((minutoBase % 60) + 60) % 60;
      prog.horariosTreino.push({ dia: nomeDia, hora: hora, minuto: minuto, data: new Date().toISOString() });
      contadorHorario++;
    });
  });

  prog.nutricao = {};
  const metaAguaSeed = calcularMetaAguaLitros(a.peso);
  const nutricaoSemanal = [
    { fugas: 1, tipos: ['mais'], alcool: 0, agua: 2.8, cardio: 90, sono: 7.5 },
    { fugas: 0, tipos: [], alcool: 2, agua: 2.5, cardio: 120, sono: 7 },
    { fugas: 2, tipos: ['doce'], alcool: 0, agua: 2.0, cardio: 0, sono: 6.5 },
    { fugas: 1, tipos: ['mais'], alcool: 3, agua: 1.8, cardio: 60, sono: 5.5 },
    { fugas: 3, tipos: ['doce', 'menos'], alcool: 0, agua: 1.6, cardio: 0, sono: 5 },
    { fugas: 1, tipos: ['mais'], alcool: 0, agua: 2.6, cardio: 100, sono: 7 },
    { fugas: 2, tipos: ['doce'], alcool: 2, agua: 2.2, cardio: 40, sono: 6 }
  ];
  nutricaoSemanal.forEach(function(dado, idx){
    const semana = idx + 1;
    const boaConstancia = (prog.diasConcluidos[semana] || []).length >= totalDiasDeTreino();
    const semanasAnteriores = [semana - 1, semana - 2].filter(function(s){ return prog.nutricao[s]; });
    const alcoolFrequente = dado.alcool > 0 && semanasAnteriores.filter(function(s){ return prog.nutricao[s].resultado.dosesAlcool > 0; }).length >= 1;
    prog.nutricao[semana] = { fugas: dado.fugas, tipos: dado.tipos, resultado: calcularNutricaoSemana(dado.fugas, dado.tipos, boaConstancia, dado.alcool, alcoolFrequente, dado.agua, metaAguaSeed, dado.cardio, dado.sono) };
  });

  a.cicloPerguntado = true;
  a.cicloInfo = { diasAtras: 14, medicamento: 'Não', semanaEstimadaProximoPeriodo: 7 };

  atualizarHomeEstatico(); // mantido por segurança, mas quem realmente atualiza a tela agora é renderHome
  renderHome();
}

function atualizarHomeEstatico(){
  const stats = calcularEstatisticasAluna(NOME_ALUNA_LOGADA);
  if(!stats.temDados) return;
  const eyebrow = document.getElementById('home-hero-eyebrow');
  const title = document.getElementById('home-hero-title');
  const volume = document.getElementById('home-volume-valor');
  const seq = document.getElementById('home-sequencia-valor');
  const seqMeta = document.getElementById('home-sequencia-meta');
  const pct = Math.round((stats.totalConcluidos / stats.totalPlanejado) * 100);
  if(eyebrow) eyebrow.textContent = 'Sua constância até agora';
  if(title) title.textContent = pct + '% dos treinos concluídos';
  if(volume) volume.textContent = stats.totalConcluidos + ' / ' + stats.totalPlanejado;
  if(seq) seq.textContent = stats.progressoes + ' progressões';
  if(seqMeta) seqMeta.textContent = 'de carga registradas até agora';
}

simularHistorico40Dias();

function exportarRelatorioEvolucao(nome){
  const a = alunasPersonal.find(function(x){ return x.nome === nome; });
  if(!a) return;
  const prog = getProgressoAluna(nome);
  const stats = calcularEstatisticasAluna(nome);
  const nutriStats = calcularNutricaoStats(nome);
  const probabilidade = calcularProbabilidadeSucesso(nome);
  const elegibilidade = calcularElegibilidadeFase(nome);
  const hoje = new Date().toLocaleDateString('pt-BR');

  let html = '<!DOCTYPE html><html lang="pt-BR"><head><meta charset="UTF-8"><title>Relatório de Evolução, ' + a.nome + '</title>';
  html += '<style>' +
    'body{font-family:Georgia,serif;background:#fff;color:#1a1a1a;max-width:720px;margin:40px auto;padding:0 24px;}' +
    'h1{color:#4C3E25;font-size:26px;margin-bottom:4px;}' +
    'h2{color:#4C3E25;font-size:17px;border-bottom:1px solid #E8C58A;padding-bottom:6px;margin-top:32px;}' +
    '.meta{color:#666;font-size:13px;margin-bottom:24px;}' +
    'table{width:100%;border-collapse:collapse;margin-top:8px;}' +
    'td,th{padding:6px 8px;border-bottom:1px solid #eee;text-align:left;font-size:13px;}' +
    'th{color:#4C3E25;}' +
    '.badge{display:inline-block;background:#F1DE9A;color:#5a4a12;padding:3px 10px;border-radius:6px;font-size:12px;}' +
    '.ok{color:#0F6E56;} .pendente{color:#999;}' +
    '@media print{ body{margin:0;} }' +
    '</style></head><body>';

  html += '<h1>MUSA+, Relatório de Evolução</h1>';
  html += '<p class="meta">' + a.nome + ' · gerado em ' + hoje + '</p>';

  html += '<h2>Resumo geral</h2><table>' +
    '<tr><td>Nível</td><td>' + a.nivel + '</td></tr>' +
    '<tr><td>Frequência</td><td>' + a.freq + '</td></tr>' +
    (a.treinoAtual ? '<tr><td>Fase atual</td><td>' + a.treinoAtual.fase + '</td></tr>' : '') +
    '<tr><td>Semana atual do ciclo</td><td>' + prog.semana + '</td></tr>' +
    (probabilidade !== null ? '<tr><td>Probabilidade de sucesso</td><td>' + probabilidade + '%</td></tr>' : '') +
    '</table>';

  if(stats.temDados){
    const pct = Math.round((stats.totalConcluidos / stats.totalPlanejado) * 100);
    html += '<h2>Constância</h2><table>' +
      '<tr><td>Treinos concluídos</td><td>' + stats.totalConcluidos + ' de ' + stats.totalPlanejado + ' (' + pct + '%)</td></tr>' +
      '<tr><td>Progressões de carga registradas</td><td>' + stats.progressoes + '</td></tr>' +
      '</table>';
  }

  html += '<h2>Progressão de carga por exercício</h2>';
  const nomesExercicios = Object.keys(prog.historico);
  if(nomesExercicios.length === 0){
    html += '<p>Nenhuma sessão registrada ainda.</p>';
  }
  nomesExercicios.forEach(function(nomeEx){
    html += '<p style="margin-bottom:2px;"><b>' + nomeEx + '</b></p><table>';
    prog.historico[nomeEx].forEach(function(r){
      html += '<tr><td>Semana ' + r.semana + '</td><td>' + r.carga + 'kg × ' + r.reps + ' reps</td><td>' + r.sugestao.texto + '</td></tr>';
    });
    html += '</table>';
  });

  if(nutriStats.temDados){
    html += '<h2>Nutrição e estilo de vida (média do período)</h2><table>' +
      '<tr><td>Resposta Nutricional</td><td>' + nutriStats.respostaNutricional + '</td></tr>' +
      '<tr><td>Potencial de Hipertrofia</td><td>' + nutriStats.potencialHipertrofia + '</td></tr>' +
      '<tr><td>Potencial de Ganho de Gordura</td><td>' + nutriStats.potencialGanhoGordura + '</td></tr>' +
      '<tr><td>Retenção Hídrica Estimada</td><td>' + nutriStats.retencaoHidricaEstimada + '</td></tr>' +
      '<tr><td>Semanas com consumo de álcool relatado</td><td>' + nutriStats.semanasComAlcool + ' de ' + nutriStats.semanasRespondidas + '</td></tr>' +
      '<tr><td>Semanas com cardio relatado</td><td>' + nutriStats.semanasComCardio + ' de ' + nutriStats.semanasRespondidas + '</td></tr>' +
      '</table>';
  }

  if(a.pesoHistorico && a.pesoHistorico.length){
    html += '<h2>Histórico de peso</h2><table><tr><th>Semana</th><th>Peso</th></tr>';
    a.pesoHistorico.forEach(function(p){
      html += '<tr><td>' + p.semana + '</td><td>' + p.peso + 'kg</td></tr>';
    });
    html += '</table>';
  }

  if(elegibilidade){
    html += '<h2>Avaliação de mudança de fase</h2><table>';
    elegibilidade.criterios.forEach(function(c){
      html += '<tr><td>' + c.nome + '</td><td class="' + (c.atingido ? 'ok' : 'pendente') + '">' + (c.atingido ? '✓' : '○') + ' ' + c.detalhe + '</td></tr>';
    });
    html += '</table><p>' + (elegibilidade.elegivel ? '<span class="badge">Elegível para avançar de fase</span>' : 'Ainda não elegível, critérios pendentes acima.') + '</p>';
  }

  html += '<p class="meta" style="margin-top:40px;">Relatório gerado automaticamente pelo MUSA+ com base nos dados registrados no aplicativo.</p>';
  html += '</body></html>';

  const janela = window.open('', '_blank');
  if(janela){
    janela.document.write(html);
    janela.document.close();
  }
}

function resetarDadosTeste(){
  const nome = NOME_ALUNA_LOGADA;
  const a = alunasPersonal.find(function(x){ return x.nome === nome; });
  progressoesPorAluna[nome] = { semana: 1, historico: {}, diasConcluidos: {}, substituicoes: [] };
  if(a){ a.cicloPerguntado = false; a.cicloInfo = null; }

  const eyebrow = document.getElementById('home-hero-eyebrow');
  const title = document.getElementById('home-hero-title');
  const volume = document.getElementById('home-volume-valor');
  const seq = document.getElementById('home-sequencia-valor');
  const seqMeta = document.getElementById('home-sequencia-meta');
  if(eyebrow) eyebrow.textContent = 'Seu primeiro dia';
  if(title) title.textContent = 'Seu DNA MUSA está calibrando';
  if(volume) volume.textContent = '0 / 20';
  if(seq) seq.textContent = 'Dia 1';
  if(seqMeta) seqMeta.textContent = 'Sua jornada começa agora';
  renderHome();

  alert('Dados de teste zerados. A Andriele volta ao dia 1.');
  if(alunaAberta){ openAlunaDetail(alunasPersonal.indexOf(alunaAberta)); }
}

let veioDaListaDeTreinosDaSemana = false;
async function sairDeVerdade(){
  try {
    if(supabaseClient) await supabaseClient.auth.signOut();
  } catch(e){}
  try { localStorage.clear(); sessionStorage.clear(); } catch(e){}
  window.location.reload();
}

function goBack(){
  const detailActive = document.querySelector('[data-view="detail"]').classList.contains('active');
  if(detailActive && veioDaListaDeTreinosDaSemana){
    veioDaListaDeTreinosDaSemana = false;
    setActive('semana-treinos');
    document.getElementById('backlabel').textContent = 'Voltar para o início';
  } else if(detailActive){
    setActive(level2);
    document.getElementById('backlabel').textContent = 'Voltar para o início';
  } else if(document.getElementById('backlabel').textContent === 'Sair'){
    sairDeVerdade();
  } else {
    setActive('launcher');
    document.getElementById('backbar').style.display = 'none';
  }
}
</script>

</body>
</html>
