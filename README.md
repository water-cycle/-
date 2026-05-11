import { useEffect, useState } from "react";

const PASSWORD = "000123";

export default function SharedMemoryVault() {
  const [unlocked, setUnlocked] = useState(false);
  const [input, setInput] = useState("");
  const [files, setFiles] = useState<any[]>([]);

  useEffect(() => {
    const saved = localStorage.getItem("vault-unlocked");
    if (saved === "true") {
      setUnlocked(true);
    }
  }, []);

  const handleUnlock = () => {
    if (input === PASSWORD) {
      localStorage.setItem("vault-unlocked", "true");
      setUnlocked(true);
    } else {
      alert("Wrong password");
    }
  };

  const handleUpload = (e: any) => {
    const selected = Array.from(e.target.files);

    const mapped = selected.map((file: any) => ({
      name: file.name,
      type: file.type.startsWith("video") ? "video" : "image",
      url: URL.createObjectURL(file)
    }));

    setFiles((prev) => [...prev, ...mapped]);
  };

  if (!unlocked) {
    return (
      <div className="min-h-screen bg-black flex items-center justify-center px-6">
        <div className="w-full max-w-md bg-[#17090b] border border-[#5c1e28] rounded-3xl p-8 shadow-2xl">
          <h1 className="text-4xl font-bold text-[#d44a5c] text-center mb-3">
            Memory Vault
          </h1>

          <p className="text-gray-400 text-center mb-8">
            Enter password to access the private gallery.
          </p>

          <input
            type="password"
            placeholder="Password"
            value={input}
            onChange={(e) => setInput(e.target.value)}
            className="w-full bg-[#2a1115] border border-[#6d2430] rounded-2xl px-4 py-3 text-white outline-none mb-4"
          />

          <button
            onClick={handleUnlock}
            className="w-full bg-[#8f2334] hover:bg-[#b42e45] transition rounded-2xl py-3 font-medium"
          >
            Enter
          </button>
        </div>
      </div>
    );
  }

  const photos = [
  const photos = [
    {
      type: "image",
      url: "https://images.unsplash.com/photo-1517841905240-472988babdf9?q=80&w=1200&auto=format&fit=crop",
      title: "Late Night"
    },
    {
      type: "image",
      url: "https://images.unsplash.com/photo-1511988617509-a57c8a288659?q=80&w=1200&auto=format&fit=crop",
      title: "City Lights"
    },
    {
      type: "video",
      url: "https://www.w3schools.com/html/mov_bbb.mp4",
      title: "Shared Video"
    }
  ];

  return (
    <div className="min-h-screen bg-gradient-to-b from-[#120607] via-[#1a090b] to-black text-white p-6">
      <div className="max-w-6xl mx-auto">
        <header className="flex flex-col md:flex-row md:items-center md:justify-between gap-4 mb-10">
          <div>
            <h1 className="text-4xl md:text-5xl font-bold tracking-tight text-[#d44a5c]">
              Memory Vault
            </h1>
            <p className="text-gray-300 mt-2 text-sm md:text-base">
              A private space for photos, videos, and shared memories.
            </p>
          </div>

          <button className="bg-[#7f1d2d] hover:bg-[#9f2b3d] transition px-5 py-3 rounded-2xl shadow-lg border border-[#c44b5d]">
            Upload Files
          </button>
        </header>

        <section className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
          {photos.map((item, index) => (
            <div
              key={index}
              className="bg-[#1b0d10] border border-[#4a1d24] rounded-3xl overflow-hidden shadow-2xl hover:scale-[1.02] transition"
            >
              {item.type === "image" ? (
                <img
                  src={item.url}
                  alt={item.title}
                  className="w-full h-72 object-cover"
                />
              ) : (
                <video
                  controls
                  className="w-full h-72 object-cover bg-black"
                >
                  <source src={item.url} type="video/mp4" />
                </video>
              )}

              <div className="p-4 flex items-center justify-between">
                <h2 className="font-medium text-lg text-[#f3d6da]">
                  {item.title}
                </h2>

                <span className="text-xs px-3 py-1 rounded-full bg-[#4f1620] text-[#ffb6c1]">
                  {item.type}
                </span>
              </div>
            </div>
          ))}
        </section>

        <section className="mt-14 grid md:grid-cols-3 gap-6">
          <div className="bg-[#1a0d0f] rounded-3xl border border-[#4a1d24] p-6">
            <h3 className="text-xl font-semibold text-[#e86c7d] mb-3">
              Shared Storage
            </h3>
            <p className="text-gray-300 text-sm leading-relaxed">
              Store photos and long videos together in one private space.
            </p>
          </div>

          <div className="bg-[#1a0d0f] rounded-3xl border border-[#4a1d24] p-6">
            <h3 className="text-xl font-semibold text-[#e86c7d] mb-3">
              Minimal Design
            </h3>
            <p className="text-gray-300 text-sm leading-relaxed">
              Clean dark-red aesthetic focused on memories instead of clutter.
            </p>
          </div>

          <div className="bg-[#1a0d0f] rounded-3xl border border-[#4a1d24] p-6">
            <h3 className="text-xl font-semibold text-[#e86c7d] mb-3">
              Video Friendly
            </h3>
            <p className="text-gray-300 text-sm leading-relaxed">
              Designed to support large uploads like 2-hour videos.
            </p>
          </div>
        </section>

        <footer className="mt-16 text-center text-gray-500 text-sm">
          Built for two people, shared forever.
        </footer>
      </div>
    </div>
  );
}

/*
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
HOW TO MAKE THIS A REAL WEBSITE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

1. Create a free account on Supabase:
   https://supabase.com

2. Create a Storage Bucket named:
   memories

3. Connect Supabase to this React app.

4. Deploy the website for free using:
   - Vercel
   - Netlify

5. Recommended setup:
   Frontend: React + Tailwind
   Storage: Supabase Storage
   Database: Supabase Postgres

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
WHY SUPABASE?
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

- Easy uploads
- Handles long videos
- Private accounts
- Shared storage
- Free tier is generous
- Much simpler than building your own server

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
FEATURES YOU CAN ADD LATER
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

- Password login
- Shared calendar
- Private notes
- Music player
- Timeline view
- Drag-and-drop upload
- Automatic video compression
- Download button
- Secret hidden pages
- Mobile app version

*/
